---
title: Observer
description: Test observability for Playwright pipelines. Follow runs live and investigate failures, attempts, steps, attachments, and historical trends.
mermaid: true
---

{{< blocks/cover title="Test observability" image_anchor="top" height="min" >}}

<p class="lead mt-3">For your automation pipeline: follow Playwright runs live, then investigate failures, retries, steps, timing, metadata, and attachments.</p>
<a class="btn btn-lg btn-primary me-3 mb-3" href="https://observer.rocks">
Try the Live Demo <i class="fas fa-arrow-alt-circle-right ms-2"></i>
</a>
<a class="btn btn-lg btn-secondary me-3 mb-3" href="/docs/getting-started/">
Get Started <i class="fas fa-arrow-alt-circle-right ms-2"></i>
</a>
<p class="mb-0">Open source and self-hostable. <a href="https://github.com/stanterprise/observer">Explore Observer on GitHub</a>.</p>
{{< /blocks/cover >}}

{{% blocks/section color="white" %}}

## From end-of-run reports to test observability

Traditional test reports summarize results after a run completes. Observer receives test events during execution, so teams can follow progress as it happens, inspect failures in context, and compare behavior across runs.
{{% /blocks/section %}}

{{% blocks/section color="dark" type="row" %}}
{{% blocks/feature icon="fa-wave-square" title="Follow runs live" %}}
Track run and test status while the suite is executing, with timing and CI metadata alongside the results.
{{% /blocks/feature %}}

{{% blocks/feature icon="fa-list-check" title="Investigate each attempt" %}}
Move from run summaries into test attempts, nested steps, durations, failures, and attached evidence.
{{% /blocks/feature %}}

{{% blocks/feature icon="fa-chart-line" title="Test Analytics" %}}
Review execution duration and outcomes across runs to spot changes in test behavior over time.
{{% /blocks/feature %}}

{{% blocks/feature icon="fa-code-branch" title="Connect your pipeline" %}}
Use the Playwright reporter today. Pytest and Mocha reporter work is in progress.
{{% /blocks/feature %}}

{{% /blocks/section %}}

{{% blocks/section color="white" %}}

## See the run, then inspect the test

Explore a run list and a detailed test execution view from the public demo. The screenshots show sample product data.

<div class="row g-4 align-items-start">
    <div class="col-lg-7">
        <figure class="figure w-100">
            <img class="figure-img img-fluid rounded border" src="/images/product/observer-run-history.png" alt="Observer run history listing test runs with status, duration, and passed, flaky, failed, and skipped counts.">
            <figcaption class="figure-caption">Run history shows outcomes and duration across recent executions.</figcaption>
        </figure>
    </div>
    <div class="col-lg-5">
        <figure class="figure w-100">
            <img class="figure-img img-fluid rounded border" src="/images/product/observer-test-details-steps.png" alt="Observer test detail showing a passed attempt, execution timing, hooks, and named test steps.">
            <figcaption class="figure-caption">Test details connect attempt status and timing to the steps that ran.</figcaption>
        </figure>
    </div>
</div>
{{% /blocks/section %}}

{{% blocks/section color="white" %}}

## How Observer works

Test events flow through a durable processing pipeline. PostgreSQL stores run data for API queries; MongoDB is limited to buffering in-flight steps.

<pre class="mermaid">
graph LR
        Tests[Playwright tests] --> Reporter[Playwright reporter]
        Reporter -->|gRPC events| Ingestion[gRPC ingestion]
        Ingestion --> NATS[NATS JetStream]
        NATS --> Processor[Processor]
        Processor -->|Durable run data| PostgreSQL[(PostgreSQL)]
        Processor -->|In-flight steps| MongoDB[(MongoDB buffer)]
        PostgreSQL --> API[API and WebSocket]
        NATS -.->|Live event relay| API
        API --> UI[Observer web UI]
</pre>

{{% /blocks/section %}}

{{% blocks/section color="primary" %}}

## Put your next test run in view

<a class="btn btn-lg btn-light me-3 mb-3" href="https://observer.rocks">
    Try the Live Demo <i class="fas fa-arrow-alt-circle-right ms-2"></i>
</a>
<a class="btn btn-lg btn-secondary me-3 mb-3" href="/docs/getting-started/">
    Run Observer Locally <i class="fas fa-arrow-alt-circle-right ms-2"></i>
</a>
<a class="btn btn-lg btn-outline-light mb-3" href="/docs/">
    Explore Documentation <i class="fas fa-arrow-alt-circle-right ms-2"></i>
</a>
{{% /blocks/section %}}
