---
title: Documentation
linkTitle: Docs
---

# Observer Documentation

Welcome to the Observer documentation. Observer is a test observability system for Playwright pipelines that ingests execution events over gRPC, processes them through NATS JetStream, and exposes run data through a REST API and Web UI.

## Getting Started

New to Observer? Start here:

- [Getting Started Guide](/docs/getting-started/) - Quick start guide to get Observer up and running
- [Installation](/docs/install/) - Deployment instructions for Docker and Kubernetes/Helm
- [Architecture](/docs/architecture/) - Understanding Observer's distributed architecture
- [Integrations](/docs/integrations/) - Connect Observer with Playwright and your CI/CD pipeline
- [Playwright Reporter](/docs/integrations/playwright-reporter/) - Detailed guide for the Playwright reporter
- [Demo](/docs/demo/) - Run a realistic local demo flow with Playwright
- [Roadmap](/docs/roadmap/) - Planning scaffold for upcoming Observer milestones

## Key Features

Observer provides comprehensive test observability for modern CI/CD pipelines:

- **Real-Time Test Monitoring**: Track test execution in real-time with WebSocket streaming
- **Run and Test Analytics**: Inspect run-level status, test outcomes, and trends
- **Step-by-Step Tracking**: Monitor individual test steps with timing and status information
- **Attachment Management**: Automatically handle screenshots, videos, and trace files
- **Sharding Support**: Aggregate results from parallel test execution
- **Flexible Deployment**: Choose between All-in-One mode for simplicity or Distributed mode for scalability

## Architecture

Observer is built on a modern, event-driven architecture:

- **Ingestion Service**: gRPC endpoint for receiving test events
- **NATS JetStream**: Message broker for reliable event streaming
- **Processor Service**: Consumes events and persists durable run data to PostgreSQL
- **API Service**: Provides REST endpoints and WebSocket streaming
- **Web UI**: React-based dashboard for visualizing test runs
- **MongoDB (limited scope)**: Live in-flight step buffering only

## Quick Links

### For Developers

- [Playwright Reporter Configuration](/docs/integrations/playwright-reporter/) - Configure the reporter in your tests
- [CI/CD Integration](/docs/integrations/#cicd-integrations) - Integrate with GitHub Actions, GitLab CI, Jenkins

### For DevOps

- [Docker Deployment](/docs/install/) - Run Observer with Docker
- [Kubernetes/Helm](/docs/install/) - Deploy Observer on Kubernetes
- [Configuration](/docs/integrations/#observer-configuration) - Configure reporter and services

## Repositories

- **Observer**: [github.com/stanterprise/observer](https://github.com/stanterprise/observer)
- **Playwright Reporter**: [github.com/stanterprise/stanterprise-playwright-reporter](https://github.com/stanterprise/stanterprise-playwright-reporter)
- **NPM Package**: [npmjs.com/package/@stanterprise/playwright-reporter](https://www.npmjs.com/package/@stanterprise/playwright-reporter)

## Support

Need help? Check out our [Community](/community/) page for support options.
