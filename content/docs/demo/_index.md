---
title: Demo
weight: 5
description: Run a practical local demo of Observer
---

# Observer Demo

Use this page to run a realistic local demo of Observer with Playwright tests.

## Read-Only Hosted Demo

A public read-only demo is available at:

- [https://observer.rocks](https://observer.rocks)

Use it to explore the Observer UI and sample run data without changing system state.

### What to Inspect in the Hosted Demo

- Run history, pass/fail status, and execution duration.
- A run's test details, attempts, and step-level information.
- CI and run metadata where the sample includes it.
- Retries and attachments on runs that contain them.

The hosted data is read-only and representative, not a complete fixture of every Observer capability. Some details, such as attachments or retries, appear only when present in a sample run.

## Local Demo Option

For full control and your own test data, run Observer locally.

## Local Demo (AIO)

Start Observer in All-in-One mode:

```bash
docker run -d \
  --name observer \
  -p 3000:80 \
  -p 50051:50051 \
  -p 5432:5432 \
  -v observer-data:/data \
  ghcr.io/stanterprise/observer/aio:latest
```

Then open the web UI at http://localhost:3000.

## Generate Demo Data with Playwright

In any Playwright project, install the reporter:

```bash
npm install --save-dev @stanterprise/playwright-reporter
```

Update `playwright.config.ts`:

```typescript
import { defineConfig } from "@playwright/test";

export default defineConfig({
  reporter: [
    ["list"],
    [
      "@stanterprise/playwright-reporter",
      {
        grpcAddress: "localhost:50051",
        tls: false,
        verbose: true,
      },
    ],
  ],
});
```

Run tests:

```bash
npx playwright test
```

You should see the run and tests appear in the Observer UI while the suite executes.

## What to Validate During the Demo

- Run list updates in near real time
- Test status transitions (running, passed, failed)
- Step-level details for tests
- Attachments and metadata reported by the Playwright reporter

## Optional: Verify API and WebSocket Endpoints

```bash
# REST (in distributed mode with API exposed on 8080)
curl http://localhost:8080/api/runs

# WebSocket endpoint for real-time relay
# ws://localhost:8080/ws
```

## Distributed Demo (Optional)

For multi-service validation, run the distributed profile from the Observer repository:

```bash
docker compose --profile dist up -d
```

Then point the reporter to the ingestion endpoint (`localhost:50051` if port-forwarded or exposed).

## Next Steps

- [Getting Started](/docs/getting-started/) for end-to-end setup
- [Architecture](/docs/architecture/) for component and data-flow details
- [Playwright Reporter Guide](/docs/integrations/playwright-reporter/) for full reporter configuration
