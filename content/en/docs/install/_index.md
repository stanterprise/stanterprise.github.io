---
title: Installation
weight: 2
description: Detailed installation instructions for Observer
---

# Installing Observer

Observer can be installed in multiple ways to suit your environment and requirements.

## Installation Methods

Choose the installation method that best fits your needs:

### Kubernetes (Recommended)

Observer is designed to run on Kubernetes and provides the best experience in this environment.

#### Using Helm

The recommended way to install Observer on Kubernetes:

```bash
# Add repository
helm repo add observer https://charts.observer.io
helm repo update

# Install with default values
helm install observer observer/observer \
  --namespace observer \
  --create-namespace

# Or install with custom values
helm install observer observer/observer \
  --namespace observer \
  --create-namespace \
  --values custom-values.yaml
```

#### Using kubectl

You can also install using raw Kubernetes manifests:

```bash
# Apply the manifests
kubectl apply -f https://raw.githubusercontent.com/observer/observer/main/deploy/kubernetes/observer.yaml
```

### Docker

For local development or testing:

```bash
# Run Observer with Docker Compose
docker-compose up -d

# Or run individual containers
docker run -d \
  --name observer \
  -p 8080:8080 \
  observer/observer:latest
```

### Binary Installation

For non-containerized environments:

```bash
# Download the latest release
curl -LO https://github.com/observer/observer/releases/latest/download/observer-linux-amd64

# Make it executable
chmod +x observer-linux-amd64

# Move to PATH
sudo mv observer-linux-amd64 /usr/local/bin/observer

# Run Observer
observer start
```

## Configuration

### Basic Configuration

Create a configuration file `observer.yaml`:

```yaml
# Server configuration
server:
  port: 8080
  host: 0.0.0.0

# Storage configuration
storage:
  type: postgresql
  connection: postgres://user:pass@localhost/observer

# Data retention
retention:
  metrics: 30d
  traces: 7d
  logs: 14d
```

### Advanced Configuration

For production deployments, consider:

- **High Availability**: Run multiple replicas
- **Resource Limits**: Set appropriate CPU/memory limits
- **Security**: Enable TLS and authentication
- **Backup**: Configure regular backups

Example production `values.yaml` for Helm:

```yaml
replicaCount: 3

resources:
  limits:
    cpu: 2000m
    memory: 4Gi
  requests:
    cpu: 500m
    memory: 1Gi

persistence:
  enabled: true
  size: 100Gi

ingress:
  enabled: true
  hosts:
    - host: observer.example.com
      paths:
        - /
  tls:
    - secretName: observer-tls
      hosts:
        - observer.example.com
```

## Verification

After installation, verify Observer is running:

```bash
# Check pod status (Kubernetes)
kubectl get pods -n observer

# Check container status (Docker)
docker ps | grep observer

# Check service status (Binary)
systemctl status observer
```

## Upgrading

To upgrade to a new version:

```bash
# Helm
helm upgrade observer observer/observer \
  --namespace observer

# Docker
docker pull observer/observer:latest
docker-compose restart

# Binary
curl -LO https://github.com/observer/observer/releases/latest/download/observer-linux-amd64
sudo mv observer-linux-amd64 /usr/local/bin/observer
systemctl restart observer
```

## Troubleshooting

### Pods Not Starting

Check pod logs:

```bash
kubectl logs -n observer <pod-name>
```

### Connection Issues

Verify network policies and service configurations:

```bash
kubectl describe service -n observer observer-api
```

### Performance Issues

Monitor resource usage:

```bash
kubectl top pods -n observer
```

## Next Steps

- [Architecture Overview](/docs/architecture/) - Understand Observer's components
- [Getting Started](/docs/getting-started/) - Quick start guide
- [Integrations](/docs/integrations/) - Connect to your data sources
