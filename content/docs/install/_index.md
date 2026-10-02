---
title: Installation
weight: 2
description: Detailed installation instructions for Observer
---

# Installing Observer

Observer can run in two deployment styles:

- **All-in-One (AIO)**: single container for local development and lightweight CI usage
- **Distributed**: separate services for ingestion, processor, API, web, and infrastructure

## Installation Methods

Choose the method that best matches your environment.

### 1. Docker (AIO)

Fastest way to run Observer locally:

```bash
docker run -d \
  --name observer \
  -p 3000:80 \
  -p 50051:50051 \
  -p 5432:5432 \
  -v observer-data:/data \
  ghcr.io/stanterprise/observer/aio:latest
```

This exposes:

- Web UI: http://localhost:3000
- gRPC ingestion: localhost:50051
- PostgreSQL (for local inspection): localhost:5432

### 2. Kubernetes with Helm

Deploy from the OCI chart:

```bash
helm install observer oci://ghcr.io/stanterprise/observer/charts/observer --version 0.1.0

# Optional local access during testing
kubectl port-forward svc/observer-web 3000:80
kubectl port-forward svc/observer-ingestion 50051:50051
```

### 3. Distributed Docker Compose

For local multi-service testing:

```bash
docker compose --profile dist up -d
```

## Configuration

### Ingestion service

- `PORT` (default `50051`)
- `NATS_URL`
- `NATS_STREAM`
- `NATS_SUBJECT_PREFIX`

### Processor service

- `POSTGRES_DSN` or `DATABASE_URL` (primary persistence)
- `MONGODB_URI` or `MONGO_URI` (live step buffering)
- `NATS_URL`, `NATS_STREAM`, `NATS_CONSUMER`

### API service

- `PORT` (default `8080`)
- `POSTGRES_DSN` or `DATABASE_URL` (required for REST reads)
- `NATS_URL`, `NATS_STREAM`, `NATS_WS_CONSUMER` (WebSocket relay)

## Verification

After installation, verify Observer is running:

```bash
# Docker/AIO
docker ps | grep observer

# Kubernetes
kubectl get pods

# API through the AIO web proxy
curl http://localhost:3000/api/runs
```

## Troubleshooting

### AIO container is up but no events appear

- Confirm reporter points to `localhost:50051`
- Confirm tests are running with `@stanterprise/playwright-reporter`

```bash
docker logs observer
```

### API has no data in distributed mode

- Verify processor has PostgreSQL and NATS connectivity
- Verify API has PostgreSQL connectivity

```bash
docker compose logs processor
docker compose logs api
```

## Next Steps

- [Architecture Overview](/docs/architecture/) - Understand Observer's components
- [Getting Started](/docs/getting-started/) - Quick start guide
- [Integrations](/docs/integrations/) - Connect Playwright and CI pipelines
