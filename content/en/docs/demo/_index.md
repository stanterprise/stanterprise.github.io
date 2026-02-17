---
title: Demo
weight: 5
description: Try Observer with sample applications and data
---

# Observer Demo

Try Observer with our interactive demos and sample applications to see how it works in practice.

## Interactive Demo Environment

We provide a fully functional demo environment where you can explore Observer's features without installation.

### Access Demo Environment

Visit our live demo: [https://demo.observer.io](https://demo.observer.io)

**Demo Credentials:**
- Username: `demo@observer.io`
- Password: `demo123`

> **Note**: These are intentionally simple credentials for demo purposes only. Never use such simple passwords in production environments.

The demo environment includes:
- Pre-populated sample data from various sources
- Multiple dashboards showing different use cases
- Real-time metrics from a simulated e-commerce application
- Sample alerts and notification configurations

## Sample Applications

### 1. Microservices E-Commerce App

Deploy our sample e-commerce application to your cluster:

```bash
# Deploy the sample app
kubectl apply -f https://raw.githubusercontent.com/observer/samples/main/ecommerce/deploy.yaml

# The app includes:
# - Frontend service (React)
# - API gateway (Node.js)
# - Product service (Go)
# - Order service (Python)
# - Payment service (Java)
# - PostgreSQL database
# - Redis cache
```

This sample demonstrates:
- Service-to-service tracing
- Database query monitoring
- Cache hit/miss metrics
- API latency tracking
- Error rate monitoring

### 2. Kubernetes Cluster Monitoring

Monitor your Kubernetes cluster with Observer:

```bash
# Install the Observer agent
helm install observer-agent observer/observer-agent \
  --set observer.endpoint=http://observer-api:8080

# View cluster metrics in the Observer dashboard
# Navigate to: Dashboards > Kubernetes > Cluster Overview
```

**Included Metrics:**
- Node CPU and memory usage
- Pod resource consumption
- Persistent volume usage
- Network traffic
- Container restart counts

### 3. Distributed Tracing Demo

See distributed tracing in action:

```bash
# Deploy the tracing demo
kubectl apply -f https://raw.githubusercontent.com/observer/samples/main/tracing/deploy.yaml

# Generate sample traffic
kubectl run -it --rm load-generator \
  --image=busybox \
  --restart=Never \
  -- /bin/sh -c "while true; do wget -q -O- http://frontend:8080; done"
```

**Trace Features:**
- End-to-end request tracking
- Service dependency mapping
- Latency breakdown by service
- Error propagation tracking

## Walkthrough Scenarios

### Scenario 1: Investigating a Performance Issue

Follow this walkthrough to investigate a simulated performance issue:

1. **Identify the Problem**
   - Navigate to **Dashboards** > **Application Performance**
   - Notice elevated response times for the `/api/products` endpoint

2. **Analyze Traces**
   - Go to **Traces** > **Search**
   - Filter by: `service=api-gateway AND http.status_code=200 AND duration>1s`
   - Click on a slow trace to view details

3. **Find the Root Cause**
   - Examine the trace waterfall
   - Identify that the database query is slow
   - Check **Metrics** > **Database** for query performance

4. **Set Up Alerts**
   - Create an alert for slow database queries
   - Navigate to **Alerting** > **Create Alert**
   - Configure: `database.query.duration > 500ms for 5 minutes`

### Scenario 2: Monitoring Deployment Impact

Track the impact of a new deployment:

1. **Baseline Metrics**
   - Before deployment, note current metrics in the dashboard
   - Key metrics: request rate, error rate, latency (p50, p95, p99)

2. **Deploy New Version**
   ```bash
   kubectl set image deployment/api-gateway \
     api-gateway=observer/api-gateway:v2.0
   ```

3. **Monitor Changes**
   - Watch the **Deployment Impact** dashboard
   - Compare metrics before and after deployment
   - Check for error rate increases or latency degradation

4. **Rollback if Needed**
   ```bash
   # If issues detected, rollback
   kubectl rollout undo deployment/api-gateway
   ```

### Scenario 3: Capacity Planning

Use Observer data for capacity planning:

1. **Analyze Historical Trends**
   - Go to **Dashboards** > **Capacity Planning**
   - Review 30-day trends for CPU, memory, and request rates

2. **Forecast Resource Needs**
   - Identify growth patterns
   - Calculate resource requirements based on trends
   - Use the built-in forecasting tool

3. **Set Resource Limits**
   ```yaml
   resources:
     requests:
       cpu: 500m
       memory: 512Mi
     limits:
       cpu: 1000m
       memory: 1Gi
   ```

## Sample Dashboards

Import these pre-built dashboards:

### Application Performance Dashboard

```json
{
  "dashboard": {
    "title": "Application Performance",
    "panels": [
      {
        "title": "Request Rate",
        "type": "graph",
        "query": "rate(http_requests_total[5m])"
      },
      {
        "title": "Error Rate",
        "type": "graph",
        "query": "rate(http_requests_total{status=~\"5..\"}[5m])"
      },
      {
        "title": "Response Time (p95)",
        "type": "graph",
        "query": "histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m]))"
      }
    ]
  }
}
```

### Infrastructure Dashboard

```json
{
  "dashboard": {
    "title": "Infrastructure Overview",
    "panels": [
      {
        "title": "CPU Usage",
        "type": "graph",
        "query": "avg(node_cpu_usage_percent) by (instance)"
      },
      {
        "title": "Memory Usage",
        "type": "graph",
        "query": "avg(node_memory_usage_percent) by (instance)"
      },
      {
        "title": "Disk I/O",
        "type": "graph",
        "query": "rate(node_disk_io_time_seconds_total[5m])"
      }
    ]
  }
}
```

## Sample Queries

Try these sample queries in the Observer query interface:

### Find Slow Requests
```promql
http_request_duration_seconds{quantile="0.95"} > 1
```

### Top 10 Services by Error Rate
```promql
topk(10, rate(http_requests_total{status=~"5.."}[5m]))
```

### Database Connection Pool Usage
```promql
(database_connections_active / database_connections_max) * 100
```

### Memory Usage Trend
```promql
avg_over_time(container_memory_usage_bytes[1h])
```

## Video Tutorials

Watch our video tutorials for step-by-step guides:

- [Getting Started with Observer](https://www.youtube.com/watch?v=example) (5 minutes)
- [Creating Custom Dashboards](https://www.youtube.com/watch?v=example) (10 minutes)
- [Setting Up Alerts](https://www.youtube.com/watch?v=example) (8 minutes)
- [Distributed Tracing Deep Dive](https://www.youtube.com/watch?v=example) (15 minutes)

## Cleanup

To remove the demo applications:

```bash
# Remove sample e-commerce app
kubectl delete -f https://raw.githubusercontent.com/observer/samples/main/ecommerce/deploy.yaml

# Remove tracing demo
kubectl delete -f https://raw.githubusercontent.com/observer/samples/main/tracing/deploy.yaml

# Remove Observer agent
helm uninstall observer-agent
```

## Next Steps

- [Getting Started](/docs/getting-started/) - Install Observer in your environment
- [Architecture](/docs/architecture/) - Learn how Observer works
- [Integrations](/docs/integrations/) - Connect your tools and services
- [Community](/community/) - Join the Observer community
