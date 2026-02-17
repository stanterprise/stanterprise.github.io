---
title: Getting Started
weight: 1
description: Quick start guide to get Observer up and running
---

# Getting Started with Observer

Welcome to Observer! This guide will help you get started with Observer in just a few minutes.

## Prerequisites

Before you begin, ensure you have:

- A Kubernetes cluster (1.19+) or Docker environment
- `kubectl` or `docker` CLI installed
- Basic familiarity with observability concepts

## Quick Start

### 1. Install Observer

The easiest way to get started is using our Helm chart:

```bash
# Add the Observer Helm repository
helm repo add observer https://charts.observer.io
helm repo update

# Install Observer
helm install observer observer/observer \
  --namespace observer \
  --create-namespace
```

### 2. Verify Installation

Check that Observer pods are running:

```bash
kubectl get pods -n observer
```

You should see output similar to:

```
NAME                        READY   STATUS    RESTARTS   AGE
observer-api-xxxxx          1/1     Running   0          1m
observer-collector-xxxxx    1/1     Running   0          1m
observer-ui-xxxxx           1/1     Running   0          1m
```

### 3. Access the Dashboard

Port-forward to access the Observer dashboard:

```bash
kubectl port-forward -n observer svc/observer-ui 8080:80
```

Open your browser to http://localhost:8080

### 4. Configure Data Sources

Observer automatically discovers services in your Kubernetes cluster. To add custom data sources:

1. Navigate to **Settings** > **Data Sources**
2. Click **Add Data Source**
3. Select your source type (Prometheus, Jaeger, etc.)
4. Configure connection details

## Next Steps

Now that Observer is running, explore these topics:

- [Installation Options](/docs/install/) - Learn about different installation methods
- [Architecture](/docs/architecture/) - Understand how Observer works
- [Integrations](/docs/integrations/) - Connect Observer to your tools
- [Demo](/docs/demo/) - Try Observer with sample applications

## Getting Help

- Check our [Community](/community/) page for support options
- Visit our [GitHub repository](https://github.com/stanterprise/stanterprise.github.io) to file issues
- Join our community chat for real-time help
