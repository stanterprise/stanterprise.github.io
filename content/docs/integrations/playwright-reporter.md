---
title: Playwright Reporter
weight: 1
description: Configure and use the @stanterprise/playwright-reporter for test observability
---

# Playwright Reporter Integration

The `@stanterprise/playwright-reporter` is a custom Playwright test reporter that sends test execution events to Observer via gRPC, enabling real-time test monitoring and comprehensive test analytics.

## Features

- 📊 **Comprehensive Test Reporting**: Reports test lifecycle events including begin, end, and failures
- 🔄 **Step Tracking**: Tracks and reports individual test steps with timing and status
- 📎 **Attachment Support**: Handles screenshots, videos, and other test attachments
- 🔌 **gRPC Integration**: Communicates with Observer backend via gRPC protocol
- ⚙️ **Configurable**: Supports environment variables and configuration options
- 🛡️ **Error Resilient**: Graceful error handling that doesn't interrupt test execution
- 🔁 **Automatic Retries**: Built-in retry logic with exponential backoff for transient failures
- 🎯 **Sharding Support**: Automatically detects and reports test shard information

## Installation

```bash
npm install @stanterprise/playwright-reporter --save-dev
```

## Basic Configuration

Add the reporter to your `playwright.config.ts`:

```typescript
import { defineConfig } from "@playwright/test";

export default defineConfig({
  reporter: [
    ["list"], // Keep console output
    ["@stanterprise/playwright-reporter"],
  ],
  // ... other config
});
```

## Advanced Configuration

### Configuration Options

```typescript
import { defineConfig } from "@playwright/test";

export default defineConfig({
  reporter: [
    [
      "@stanterprise/playwright-reporter",
      {
        grpcAddress: "localhost:50051", // Observer gRPC server address
        grpcEnabled: true, // Enable/disable reporting
        grpcTimeout: 1000, // Timeout for gRPC calls (ms)
        grpcMaxMessageSize: 104857600, // Max message size (100MB)
        maxAttachmentSize: 10485760, // Max attachment size (10MB)
        grpcMaxRetries: 3, // Max retry attempts
        grpcRetryDelay: 100, // Initial retry delay (ms)
        verbose: false, // Enable verbose logging
      },
    ],
  ],
});
```

### Configuration Table

| Option               | Type    | Default           | Description                                     |
| -------------------- | ------- | ----------------- | ----------------------------------------------- |
| `grpcAddress`        | string  | `localhost:50051` | Observer gRPC server address                    |
| `grpcEnabled`        | boolean | `true`            | Enable/disable gRPC reporting                   |
| `grpcTimeout`        | number  | `1000`            | Timeout for gRPC calls in milliseconds          |
| `grpcMaxMessageSize` | number  | `104857600`       | Max message size in bytes (100MB)               |
| `maxAttachmentSize`  | number  | `10485760`        | Max attachment content size (10MB)              |
| `grpcMaxRetries`     | number  | `3`               | Maximum retry attempts for failed calls         |
| `grpcRetryDelay`     | number  | `100`             | Initial delay for retries (exponential backoff) |
| `verbose`            | boolean | `false`           | Enable verbose logging                          |

## Environment Variables

Configure the reporter using environment variables:

```bash
# Server configuration
STANTERPRISE_GRPC_ADDRESS=myserver.com:50051
STANTERPRISE_GRPC_ENABLED=true

# Custom metadata (prefix will be stripped)
STANTERPRISE_META_BUILD_ID=12345
STANTERPRISE_META_BRANCH=main
STANTERPRISE_META_COMMIT_SHA=abc123
STANTERPRISE_META_CI_PIPELINE=github-actions
```

### Custom Metadata

Any environment variable with the `STANTERPRISE_META_` prefix is automatically included in test run metadata. The prefix is stripped from the key name.

Examples:

- `STANTERPRISE_META_BUILD_ID=12345` → `BUILD_ID: 12345`
- `STANTERPRISE_META_BRANCH=main` → `BRANCH: main`
- `STANTERPRISE_META_COMMIT_SHA=abc123` → `COMMIT_SHA: abc123`

## Retry Configuration

The reporter automatically retries failed gRPC calls for transient errors using exponential backoff:

**Retryable errors:**

- `UNAVAILABLE` - Server temporarily unavailable
- `DEADLINE_EXCEEDED` - Request timeout
- `INTERNAL` - Internal server error
- `UNKNOWN` - Unknown error

**Non-retryable errors:**

- `INVALID_ARGUMENT` - Bad request format
- `NOT_FOUND` - Resource not found
- `PERMISSION_DENIED` - Access denied
- `UNAUTHENTICATED` - Authentication required
- `ALREADY_EXISTS` - Resource already exists

**Exponential backoff formula:** `grpcRetryDelay * (2 ^ attemptNumber)`

With default settings (`grpcRetryDelay: 100`, `grpcMaxRetries: 3`):

- Initial attempt: Immediate
- Retry 1: Wait 100ms (100 \* 2^0)
- Retry 2: Wait 200ms (100 \* 2^1)
- Retry 3: Wait 400ms (100 \* 2^2)

To disable retries:

```typescript
{
  grpcMaxRetries: 0; // No retries, fail fast
}
```

## Sharding Support

The reporter automatically detects and reports test sharding configuration. When running tests in sharded mode, shard information is included in the test run metadata.

### Configuring Sharding

In `playwright.config.ts`:

```typescript
export default defineConfig({
  shard: { total: 5, current: 1 },
  reporter: [["@stanterprise/playwright-reporter"]],
});
```

Or via CLI:

```bash
npx playwright test --shard=1/5
npx playwright test --shard=2/5
# ... etc
```

### Aggregating Sharded Results

**Important:** To aggregate results from all shards into a single test run, specify the same `STANTERPRISE_RUN_ID` for all shards:

```bash
# Set the same run ID for all shards
export STANTERPRISE_RUN_ID="my-test-run-123"

# Run all shards
npx playwright test --shard=1/5
npx playwright test --shard=2/5
npx playwright test --shard=3/5
npx playwright test --shard=4/5
npx playwright test --shard=5/5
```

In CI/CD environments:

```bash
# GitHub Actions
STANTERPRISE_RUN_ID="${GITHUB_RUN_ID}" npx playwright test --shard=1/5

# GitLab CI
STANTERPRISE_RUN_ID="${CI_PIPELINE_ID}" npx playwright test --shard=1/5

# Jenkins
STANTERPRISE_RUN_ID="${BUILD_ID}" npx playwright test --shard=1/5
```

Without a shared run ID, each shard creates a separate test run instead of aggregating into one.

## What Gets Reported

### Test Events

- **Test Begin**: When a test starts execution
- **Test End**: When a test completes with status (passed/failed/skipped/timedOut)
- **Test Failure**: Detailed failure information including error messages and stack traces

### Step Events

- **Step Begin**: When a test step starts
- **Step End**: When a test step completes with duration and status

### Attachments

The reporter automatically processes and sends:

- Screenshots
- Videos
- Trace files
- Any other attachments captured during test execution

### Test Run Metadata

Automatically collected metadata includes:

- Run ID (auto-generated or custom via `STANTERPRISE_RUN_ID`)
- Test count
- Start time
- Shard information (if sharding is enabled)
- Custom metadata from environment variables

## Usage Examples

### Basic Setup

```typescript
// playwright.config.ts
import { defineConfig } from "@playwright/test";

export default defineConfig({
  use: {
    screenshot: "only-on-failure",
    video: "retain-on-failure",
  },
  reporter: [["list"], ["@stanterprise/playwright-reporter"]],
});
```

### CI/CD Integration

```typescript
// playwright.config.ts
import { defineConfig } from "@playwright/test";

export default defineConfig({
  reporter: [
    ["list"],
    [
      "@stanterprise/playwright-reporter",
      {
        grpcAddress: process.env.OBSERVER_GRPC_ADDRESS || "localhost:50051",
        verbose: process.env.CI === "true",
        grpcMaxRetries: 5, // More retries in CI
      },
    ],
  ],
});
```

### GitHub Actions Example

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

### Sharded CI/CD Example

```yaml
name: E2E Tests (Sharded)
on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        shard: [1, 2, 3, 4, 5]
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3

      - name: Install dependencies
        run: npm ci

      - name: Run Playwright tests
        env:
          STANTERPRISE_GRPC_ADDRESS: observer.example.com:50051
          STANTERPRISE_RUN_ID: ${{ github.run_id }}
          STANTERPRISE_META_SHARD: ${{ matrix.shard }}
        run: npx playwright test --shard=${{ matrix.shard }}/5
```

## Troubleshooting

### gRPC Error 13 INTERNAL

This error typically occurs due to large attachments exceeding message size limits.

**Solutions:**

1. **Increase message size limits**:

   ```typescript
   {
     grpcMaxMessageSize: 104857600, // 100MB
     maxAttachmentSize: 10485760,   // 10MB
   }
   ```

2. **Reduce attachment sizes** by saving to disk:

   ```typescript
   use: {
     screenshot: 'only-on-failure',
     video: 'retain-on-failure',
     trace: 'retain-on-failure',
   },
   ```

3. **Enable verbose logging** to monitor sizes:
   ```typescript
   {
     verbose: true;
   }
   ```

### Connection Issues

If you see connection errors:

1. Verify Observer is running and accessible
2. Check the `grpcAddress` configuration
3. Ensure firewall rules allow the connection
4. Enable verbose logging for detailed errors

### Tests Not Appearing

1. Verify the reporter is in `playwright.config.ts`
2. Check that `grpcEnabled` is not `false`
3. Look for error messages in test output
4. Try with `verbose: true`

## API Types

```typescript
import type {
  StanterpriseReporterOptions,
  TestExecutionContext,
  StepExecutionContext,
} from "@stanterprise/playwright-reporter";
```

## Next Steps

- [Observer Architecture](/docs/architecture/) - Understand how Observer processes test events
- [Installation](/docs/install/) - Deploy Observer in different environments
- [Demo](/docs/demo/) - Try Observer with sample tests

## Resources

- [Reporter GitHub Repository](https://github.com/stanterprise/stanterprise-playwright-reporter)
- [Observer GitHub Repository](https://github.com/stanterprise/observer)
- [NPM Package](https://www.npmjs.com/package/@stanterprise/playwright-reporter)
