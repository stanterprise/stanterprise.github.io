---
title: Why Observer?
weight: 1
description: Why test execution benefits from live observability and cross-run investigation.
---

Automated tests produce more than a final pass or fail. They generate a stream of events: runs start, tests execute, steps succeed or fail, retries happen, and attachments capture evidence. Most test reports emphasize the result after execution has ended.

Observer treats that execution stream as something engineers can inspect while it is happening and return to later.

## See execution as it happens

Follow run and test status while a suite is in progress. When a test fails, inspect its attempt, steps, timing, metadata, and available attachments in the same execution context.

## Investigate across runs

Keep run history queryable so teams can compare outcomes and durations, recognize recurring failures, and see when test behavior changes instead of treating each report as a disposable artifact.

## A shared model for test signals

Observer's current public integration is its Playwright reporter. The goal is to make test events useful across frameworks as additional integrations mature, while keeping framework-specific details available for investigation. Pytest and Mocha reporter work is in progress; cross-framework support should not be assumed today.

## Test observability and quality intelligence

Test observability makes execution evidence visible and searchable. That evidence can help teams reason about reliability and test health, but Observer does not currently claim automated root-cause analysis or AI-generated quality recommendations.

## Explore Observer

- [Try the live demo](/docs/demo/)
- [Get started](/docs/getting-started/)
- [Read the architecture overview](/docs/architecture/)
