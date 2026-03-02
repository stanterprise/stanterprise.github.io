---
title: Documentation
linkTitle: Docs
menu:
  main:
    weight: 20
---

# Observer Documentation

Welcome to the Observer documentation! Observer is a test observability system that collects and visualizes test execution events in real-time. Here you'll find everything you need to get started with Observer and integrate it with your Playwright tests.

## Getting Started

New to Observer? Start here:

- [Getting Started Guide](/docs/getting-started/) - Quick start guide to get Observer up and running
- [Installation](/docs/install/) - Detailed installation instructions for Docker, Kubernetes, and local development
- [Architecture](/docs/architecture/) - Understanding Observer's distributed architecture
- [Integrations](/docs/integrations/) - Connect Observer with Playwright and your CI/CD pipeline
- [Playwright Reporter](/docs/integrations/playwright-reporter/) - Detailed guide for the Playwright reporter
- [Demo](/docs/demo/) - Try Observer with sample applications

## Key Features

Observer provides comprehensive test observability for modern CI/CD pipelines:

- **Real-Time Test Monitoring**: Track test execution in real-time with WebSocket streaming
- **Test Analytics**: Analyze test performance, failure patterns, and execution trends
- **Step-by-Step Tracking**: Monitor individual test steps with timing and status information
- **Attachment Management**: Automatically handle screenshots, videos, and trace files
- **Sharding Support**: Aggregate results from parallel test execution
- **Flexible Deployment**: Choose between All-in-One mode for simplicity or Distributed mode for scalability

## Architecture

Observer is built on a modern, event-driven architecture:

- **Ingestion Service**: gRPC endpoint for receiving test events
- **NATS JetStream**: Message broker for reliable event streaming
- **Processor Service**: Processes and persists events to MongoDB
- **API Service**: Provides REST/GraphQL API and WebSocket streaming
- **Web UI**: React-based dashboard for visualizing test runs

## Quick Links

### For Developers

- [Playwright Reporter Configuration](/docs/integrations/playwright-reporter/) - Configure the reporter in your tests
- [CI/CD Integration](/docs/integrations/#cicd-integrations) - Integrate with GitHub Actions, GitLab CI, Jenkins

### For DevOps

- [Docker Deployment](/docs/install/) - Run Observer with Docker
- [Kubernetes/Helm](/docs/install/) - Deploy Observer on Kubernetes
- [Configuration](/docs/integrations/#observer-configuration) - Configure Observer services

## Repositories

- **Observer**: [github.com/stanterprise/observer](https://github.com/stanterprise/observer)
- **Playwright Reporter**: [github.com/stanterprise/stanterprise-playwright-reporter](https://github.com/stanterprise/stanterprise-playwright-reporter)
- **NPM Package**: [npmjs.com/package/@stanterprise/playwright-reporter](https://www.npmjs.com/package/@stanterprise/playwright-reporter)

## Support

Need help? Check out our [Community](/community/) page for support options.
