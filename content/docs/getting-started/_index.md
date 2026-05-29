---
title: Getting Started
weight: 1
description: Quick start guide to get Observer up and running
---

# Getting Started with Observer

Welcome to Observer! This guide will help you get started with Observer in just a few minutes. Observer is a test observability system that collects and visualizes test execution events in real-time.

## Prerequisites

Before you begin, ensure you have:

- Docker (for quick start) or Kubernetes cluster (for production)
- Node.js and npm (for Playwright tests)
- Basic familiarity with Playwright testing

## Quick Start with Docker

The fastest way to get started is using the all-in-one Docker image:

### 1. Run Observer

```bash
docker run -d \
  -p 3000:80 \
  -p 50051:50051 \
  -p 5432:5432 \
  -v observer-data:/data \
  --name observer \
  ghcr.io/stanterprise/observer/aio:latest
```

This starts Observer with:

- Web UI on port 3000 (http://localhost:3000)
- gRPC endpoint on port 50051
- PostgreSQL on port 5432 (for local inspection)

### 2. Install the Playwright Reporter

In your Playwright project, install the Observer reporter:

```bash
npm install @stanterprise/playwright-reporter --save-dev
```

### 3. Configure Playwright

Add the reporter to your `playwright.config.ts`:

```typescript
import { defineConfig } from "@playwright/test";

export default defineConfig({
  reporter: [
    ["list"], // Keep the default console reporter
    [
      "@stanterprise/playwright-reporter",
      {
        grpcAddress: "localhost:50051",
        verbose: true,
      },
    ],
  ],
  // ... your other config
});
```

### 4. Run Your Tests

Run your Playwright tests as usual:

```bash
npx playwright test
```

### 5. View Results

Open http://localhost:3000 in your browser to see your test results in real-time!

You can also explore the read-only hosted demo at [https://observer.rocks](https://observer.rocks).

## Quick Start with Kubernetes/Helm

For production deployments, use Helm:

```bash
# Install from OCI registry
helm install observer oci://ghcr.io/stanterprise/observer/charts/observer --version 0.1.0

# Port forward to access the UI
kubectl port-forward svc/observer-web 3000:80
kubectl port-forward svc/observer-ingestion 50051:50051
```

Configure your Playwright tests to point to your Kubernetes service endpoint.

## Architecture Modes

Observer supports two deployment modes:

- **All-in-One (AIO)**: Single container with embedded NATS, PostgreSQL, MongoDB, ingestion, processor, API, and web
- **Distributed Mode**: Multi-container deployment with separate ingestion, processor, API, and web services

## Next Steps

Now that Observer is running, explore these topics:

- [Installation Options](/docs/install/) - Learn about different deployment methods
- [Architecture](/docs/architecture/) - Understand how Observer works
- [Integrations](/docs/integrations/) - Configure the Playwright reporter
- [Demo](/docs/demo/) - Try Observer with sample applications
- [Roadmap](/docs/roadmap/) - View upcoming planning areas

## Getting Help

- Visit the [Observer GitHub repository](https://github.com/stanterprise/observer) for issues
- Check the [Reporter GitHub repository](https://github.com/stanterprise/stanterprise-playwright-reporter) for reporter issues
- Join our [Community](/community/) for support
