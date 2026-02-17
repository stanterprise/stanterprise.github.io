---
title: Integrations
weight: 4
description: Connect Observer with your existing tools and workflows
---

# Observer Integrations

Observer integrates seamlessly with your existing tools and infrastructure. This page covers the available integrations and how to configure them.

## Monitoring & Metrics

### Prometheus

Observer is fully compatible with Prometheus metrics:

```yaml
# observer.yaml
integrations:
  prometheus:
    enabled: true
    scrape_interval: 15s
    scrape_configs:
      - job_name: 'kubernetes-pods'
        kubernetes_sd_configs:
          - role: pod
```

**Features:**
- Native PromQL support
- Automatic service discovery
- Remote write endpoint
- Alert manager integration

### Grafana

Visualize Observer data in Grafana:

1. Add Observer as a data source in Grafana
2. Configure the connection:
   - Type: Prometheus
   - URL: `http://observer-api:9090`
3. Import Observer dashboards from the community

### Datadog

Forward metrics to Datadog:

```yaml
integrations:
  datadog:
    enabled: true
    api_key: your-api-key
    site: datadoghq.com
```

## Distributed Tracing

### OpenTelemetry

Observer has native OpenTelemetry support:

```yaml
# Application configuration
exporters:
  otlp:
    endpoint: observer-collector:4317
    insecure: true
```

**Supported formats:**
- OTLP (gRPC and HTTP)
- Jaeger
- Zipkin

### Jaeger

Configure Jaeger integration:

```yaml
integrations:
  jaeger:
    enabled: true
    collector_endpoint: observer-collector:14250
    agent_endpoint: observer-agent:6831
```

## Log Management

### Fluentd

Forward logs from Fluentd to Observer:

```xml
<match **>
  @type http
  endpoint http://observer-collector:8080/v1/logs
  json_array true
  
  <format>
    @type json
  </format>
  
  <buffer>
    @type file
    path /var/log/fluentd-buffer/observer
    flush_interval 5s
  </buffer>
</match>
```

### Logstash

Use Logstash to send logs to Observer:

```ruby
output {
  http {
    url => "http://observer-collector:8080/v1/logs"
    http_method => "post"
    format => "json"
  }
}
```

### Filebeat

Configure Filebeat to forward logs:

```yaml
output.http:
  hosts: ["observer-collector:8080"]
  path: "/v1/logs"
  codec.json:
    pretty: false
```

## Cloud Platforms

### Kubernetes

Observer integrates deeply with Kubernetes:

- **Service Discovery**: Automatic discovery of services and pods
- **Metrics Collection**: Node, pod, and container metrics
- **Log Collection**: Automatic log aggregation
- **Resource Monitoring**: CPU, memory, and storage tracking

Deploy the Observer Kubernetes operator:

```bash
kubectl apply -f https://raw.githubusercontent.com/observer/observer/main/deploy/kubernetes/operator.yaml
```

### AWS

Collect data from AWS services:

```yaml
integrations:
  aws:
    enabled: true
    region: us-east-1
    services:
      - cloudwatch
      - elb
      - rds
      - lambda
```

### Google Cloud Platform

Monitor GCP resources:

```yaml
integrations:
  gcp:
    enabled: true
    project_id: your-project-id
    services:
      - compute
      - gke
      - cloud-sql
```

### Azure

Connect to Azure Monitor:

```yaml
integrations:
  azure:
    enabled: true
    tenant_id: your-tenant-id
    subscription_id: your-subscription-id
```

## Alerting & Notifications

### Slack

Send alerts to Slack:

```yaml
alerting:
  receivers:
    - name: slack
      type: slack
      webhook_url: https://hooks.slack.com/services/YOUR/WEBHOOK/URL
      channel: '#alerts'
```

### PagerDuty

Integrate with PagerDuty for incident management:

```yaml
alerting:
  receivers:
    - name: pagerduty
      type: pagerduty
      integration_key: your-integration-key
      severity: critical
```

### Email

Configure email notifications:

```yaml
alerting:
  receivers:
    - name: email
      type: email
      smtp_server: smtp.gmail.com:587
      from: alerts@observer.io
      to:
        - team@example.com
```

## CI/CD

### GitHub Actions

Monitor your CI/CD pipelines:

```yaml
# .github/workflows/ci.yml
- name: Send metrics to Observer
  run: |
    curl -X POST http://observer:8080/v1/metrics \
      -H "Content-Type: application/json" \
      -d '{"pipeline": "ci", "status": "success", "duration": 120}'
```

### Jenkins

Use the Observer Jenkins plugin:

```groovy
pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                observerMetrics(
                    name: 'build',
                    value: 1
                )
            }
        }
    }
}
```

### GitLab CI

Track GitLab CI metrics:

```yaml
after_script:
  - curl -X POST http://observer:8080/v1/metrics \
    -d "job_duration=$CI_JOB_DURATION"
```

## Databases

### PostgreSQL

Monitor PostgreSQL metrics:

```yaml
integrations:
  postgresql:
    enabled: true
    connection: postgresql://user:pass@localhost/db
    metrics:
      - connections
      - queries
      - replication
```

### MySQL

Collect MySQL metrics:

```yaml
integrations:
  mysql:
    enabled: true
    connection: mysql://user:pass@localhost:3306
```

### MongoDB

Monitor MongoDB clusters:

```yaml
integrations:
  mongodb:
    enabled: true
    connection: mongodb://localhost:27017
    databases:
      - production
```

## Message Queues

### Kafka

Monitor Kafka clusters:

```yaml
integrations:
  kafka:
    enabled: true
    brokers:
      - kafka-1:9092
      - kafka-2:9092
    metrics:
      - broker_metrics
      - topic_metrics
      - consumer_lag
```

### RabbitMQ

Collect RabbitMQ metrics:

```yaml
integrations:
  rabbitmq:
    enabled: true
    url: http://rabbitmq:15672
    username: admin
    password: admin
```

## Custom Integrations

Create custom integrations using the Observer API:

```python
import requests

def send_metric(name, value, tags=None):
    response = requests.post(
        'http://observer:8080/v1/metrics',
        json={
            'name': name,
            'value': value,
            'tags': tags or {},
            'timestamp': int(time.time())
        }
    )
    return response.json()
```

## API Documentation

For detailed API documentation, visit the [Observer API Reference](https://api-docs.observer.io).

## Next Steps

- [Getting Started](/docs/getting-started/) - Set up Observer
- [Architecture](/docs/architecture/) - Understand Observer's design
- [Demo](/docs/demo/) - Try Observer with sample integrations
