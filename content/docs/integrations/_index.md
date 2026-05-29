---
title: Integrations
weight: 4
description: Connect Observer with your test frameworks and CI/CD pipelines
---

# Observer Integrations

Observer integrates with your test frameworks to provide comprehensive test observability. This page covers the available integrations and how to configure them.

## Test Frameworks

### Playwright Reporter

The primary way to integrate Observer with your tests is through the **@stanterprise/playwright-reporter**. This custom reporter sends test execution events to Observer in real-time.

**Quick Setup:**

```bash
npm install @stanterprise/playwright-reporter --save-dev
```

```typescript
// playwright.config.ts
import { defineConfig } from "@playwright/test";

export default defineConfig({
  reporter: [
    ["list"],
    [
      "@stanterprise/playwright-reporter",
      {
        grpcAddress: "localhost:50051",
      },
    ],
  ],
});
```

**Key Features:**

- Real-time test event streaming
- Step-by-step execution tracking
- Automatic attachment handling (screenshots, videos, traces)
- Sharding support for parallel execution
- Custom metadata injection
- Retry logic with exponential backoff

📖 See the [detailed Playwright Reporter guide](/docs/integrations/playwright-reporter/) for complete configuration options and examples.

## CI/CD Integrations

### GitHub Actions

Integrate Observer with GitHub Actions workflows:

```yaml
name: E2E Tests
on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: "18"

      - name: Install dependencies
        run: npm ci

      - name: Run Playwright tests
        env:
          STANTERPRISE_GRPC_ADDRESS: observer.example.com:50051
          STANTERPRISE_META_BUILD_ID: ${{ github.run_id }}
          STANTERPRISE_META_BRANCH: ${{ github.ref_name }}
          STANTERPRISE_META_COMMIT_SHA: ${{ github.sha }}
        run: npx playwright test
```

### GitLab CI

Configure Observer in GitLab CI:

```yaml
test:
  image: mcr.microsoft.com/playwright:latest
  script:
    - npm ci
    - npx playwright test
  variables:
    STANTERPRISE_GRPC_ADDRESS: "observer.example.com:50051"
    STANTERPRISE_META_BUILD_ID: "$CI_PIPELINE_ID"
    STANTERPRISE_META_BRANCH: "$CI_COMMIT_REF_NAME"
    STANTERPRISE_META_COMMIT_SHA: "$CI_COMMIT_SHA"
```

### Jenkins

Use Observer in Jenkins pipelines:

```groovy
pipeline {
    agent any

    environment {
        STANTERPRISE_GRPC_ADDRESS = 'observer.example.com:50051'
        STANTERPRISE_META_BUILD_ID = "${BUILD_ID}"
        STANTERPRISE_META_BRANCH = "${GIT_BRANCH}"
    }

    stages {
        stage('Test') {
            steps {
                sh 'npm ci'
                sh 'npx playwright test'
            }
        }
    }
}
```

## Deployment Integrations

### Docker

Run Observer with Docker:

```bash
# All-in-One mode (development)
docker run -d \
  -p 3000:80 \
  -p 50051:50051 \
  -v observer-data:/data \
  ghcr.io/stanterprise/observer/aio:latest
```

### Kubernetes

Deploy Observer on Kubernetes using Helm:

```bash
helm install observer oci://ghcr.io/stanterprise/observer/charts/observer --version 0.1.0

# Forward ingestion service for local test clients
kubectl port-forward svc/observer-ingestion 50051:50051
```

### Docker Compose

Use Docker Compose for local development:

```yaml
version: "3.8"
services:
  observer:
    image: ghcr.io/stanterprise/observer/aio:latest
    ports:
      - "3000:80"
      - "50051:50051"
    volumes:
      - observer-data:/data

volumes:
  observer-data:
```

## Custom Metadata

Enhance your test runs with custom metadata using environment variables:

```bash
# Build information
STANTERPRISE_META_BUILD_ID=12345
STANTERPRISE_META_BUILD_URL=https://ci.example.com/build/12345

# Git information
STANTERPRISE_META_BRANCH=feature/new-feature
STANTERPRISE_META_COMMIT_SHA=abc123def456
STANTERPRISE_META_AUTHOR=john.doe@example.com

# Environment information
STANTERPRISE_META_ENVIRONMENT=staging
STANTERPRISE_META_REGION=us-west-2

# Run your tests
npx playwright test
```

All metadata with the `STANTERPRISE_META_` prefix is automatically included in test run reports.

## Architecture Components

Observer is built on a distributed architecture:

- **Ingestion Service**: Receives gRPC test events from the Playwright reporter
- **NATS JetStream**: Message broker for event streaming
- **Processor Service**: Processes events and persists canonical run data to PostgreSQL
- **API Service**: Provides REST API and WebSocket streaming
- **Web UI**: React-based dashboard for visualizing test runs
- **MongoDB**: Used only for in-flight live step buffering

## Configuration

### Reporter Configuration

Control reporter behavior through environment variables:

- `STANTERPRISE_GRPC_ADDRESS`: gRPC server address (default: `localhost:50051`)
- `STANTERPRISE_GRPC_ENABLED`: Enable/disable reporting (default: `true`)
- `STANTERPRISE_RUN_ID`: Custom run ID for aggregating sharded tests

### Observer Configuration

Configure Observer services:

**Ingestion Service:**

- `PORT`: gRPC listening port (default: `50051`)
- `NATS_URL`: NATS server URL

**Processor Service:**

- `POSTGRES_DSN` or `DATABASE_URL`: PostgreSQL connection string (primary persistence)
- `MONGODB_URI`: MongoDB connection string
- `NATS_URL`: NATS server URL

**API Service:**

- `PORT`: HTTP listening port (default: `8080`)
- `POSTGRES_DSN` or `DATABASE_URL`: PostgreSQL connection string (required for REST)
- `NATS_URL`: NATS server URL for WebSocket streaming

## Next Steps

- [Playwright Reporter Guide](/docs/integrations/playwright-reporter/) - Detailed reporter configuration
- [Architecture](/docs/architecture/) - Understand Observer's architecture
- [Installation](/docs/install/) - Deploy Observer
- [Demo](/docs/demo/) - Try Observer with sample tests
