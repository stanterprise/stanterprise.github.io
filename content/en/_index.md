---
title: Observer
---

{{< blocks/cover title="Observer" image_anchor="top" height="full" >}}
<a class="btn btn-lg btn-primary me-3 mb-4" href="/docs/getting-started/">
  Get Started <i class="fas fa-arrow-alt-circle-right ms-2"></i>
</a>
<a class="btn btn-lg btn-secondary me-3 mb-4" href="https://github.com/stanterprise/stanterprise.github.io">
  GitHub <i class="fab fa-github ms-2 "></i>
</a>
<p class="lead mt-5">Your comprehensive developer tool for modern observability</p>
{{< /blocks/cover >}}

{{% blocks/lead color="primary" %}}
Observer is a test observability system that collects test execution events via gRPC, providing real-time insights into your test runs. 
Built for modern CI/CD pipelines, Observer helps teams understand test performance, track failures, and optimize test execution.
{{% /blocks/lead %}}

{{% blocks/section color="dark" type="row" %}}
{{% blocks/feature icon="fa-lightbulb" title="Real-Time Test Monitoring" %}}
Track test execution in real-time with WebSocket streaming and comprehensive dashboards.
{{% /blocks/feature %}}

{{% blocks/feature icon="fa-code" title="Developer-First Design" %}}
Simple gRPC API, Playwright integration, and intuitive web interface for monitoring test runs.
{{% /blocks/feature %}}

{{% blocks/feature icon="fa-chart-line" title="Test Analytics" %}}
Gain insights into test performance, failure patterns, and execution trends across your CI/CD pipeline.
{{% /blocks/feature %}}

{{% blocks/feature icon="fa-plug" title="Easy Integration" %}}
Works seamlessly with Playwright tests via our custom reporter. Kubernetes and Docker ready.
{{% /blocks/feature %}}

{{% /blocks/section %}}

{{% blocks/section color="white" %}}

## Architecture Overview

Observer is built on a modern, event-driven architecture designed for scalability and real-time test monitoring.

```mermaid
graph TB
    subgraph "Test Execution"
        A[Playwright Tests]
        B[Reporter Plugin]
    end
    
    subgraph "Observer Platform"
        C[Ingestion Service<br/>gRPC]
        D[NATS JetStream]
        E[Processor Service]
        F[Database<br/>MongoDB]
        G[API Service]
    end
    
    subgraph "User Interface"
        H[Web Dashboard<br/>React]
        I[WebSocket<br/>Real-Time]
    end
    
    A --> B
    B -->|gRPC Events| C
    C --> D
    D --> E
    E --> F
    F --> G
    G --> H
    D -.->|Stream| G
    G -.->|WebSocket| I
    I --> H
    
    style C fill:#326ce5,stroke:#fff,stroke-width:2px,color:#fff
    style D fill:#326ce5,stroke:#fff,stroke-width:2px,color:#fff
    style E fill:#326ce5,stroke:#fff,stroke-width:2px,color:#fff
    style F fill:#326ce5,stroke:#fff,stroke-width:2px,color:#fff
    style G fill:#326ce5,stroke:#fff,stroke-width:2px,color:#fff
```

{{% /blocks/section %}}

{{% blocks/section color="primary" %}}

## Get Started Today

Ready to improve your observability? Get started with Observer in minutes.

<a class="btn btn-lg btn-light me-3 mb-4" href="/docs/getting-started/">
  View Documentation <i class="fas fa-arrow-alt-circle-right ms-2"></i>
</a>

{{% /blocks/section %}}
