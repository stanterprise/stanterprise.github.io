---
title: Roadmap
weight: 6
description: Current Observer roadmap themes and statuses
---

# Observer Roadmap

This page captures the current product direction for Observer. Statuses reflect current planning and may change as implementation evolves.

## Status Legend

- Planned
- In Progress
- Blocked
- Released

## Now

| Item           | Status      | Notes                                                                                             |
| -------------- | ----------- | ------------------------------------------------------------------------------------------------- |
| Pytest support | In Progress | Prioritize parity with the existing Playwright reporter flow before expanding framework coverage. |
| Mocha support  | In Progress | Reporter implementation is underway.                                                              |

## Next

| Item                       | Status  | Notes                                                                                  |
| -------------------------- | ------- | -------------------------------------------------------------------------------------- |
| MCP Server                 | Planned | Focus on a minimal, stable interface that can expose Observer data and actions safely. |
| Manager View               | Planned | Tailor the experience for higher-level run summaries and team-level reporting.         |
| JUnit 5 support            | Planned | Align the reporter model with JUnit 5 lifecycle events and metadata.                   |
| GitHub Actions integration | Planned | Keep setup lightweight so it is easy to adopt in existing CI workflows.                |

## Later

| Item                               | Notes                                                                                                                                                                          |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Executive Summary View             | High-level reporting for stakeholders                                                                                                                                          |
| OpenTelemetry integration          | Future observability export path                                                                                                                                               |
| JUnit XML importer                 | Broader test result ingestion support                                                                                                                                          |
| .NET ecosystem                     | Potential new reporter surface                                                                                                                                                 |
| Prometheus / Grafana               | Metrics export and dashboarding                                                                                                                                                |
| Datadog integration                | Third-party observability compatibility                                                                                                                                        |
| Slack integration                  | Alerting and notifications                                                                                                                                                     |
| Cypress                            | Additional test framework support                                                                                                                                              |
| AI-assisted failure clustering     | Failure root-cause inference                                                                                                                                                   |
| AI-assisted test health assessment | Analyze tests in detail to produce optimization an improvement suggestions                                                                                                     |
| Duration-based shard balancing     | TS Playwright-specific: receive an input of tests that are intended to be run and a desired number of shards, return shard test files balanced by execution time and not count |
| CI/CD cost optimization            | Provide recommendations on CI/CD configuration to reduce execution time and save on infra costs                                                                                |

## Cross-Cutting Themes

- Reliability and correctness
- Developer experience
- Deployment and operations
- Performance and scale
- Security and hardening

## Dependencies and Risks

- Dependency: External reporter and framework support maturity
  - Impact: May affect target dates for new integrations
  - Mitigation: Keep roadmap items modular and independently shippable

- Risk: Scope growth in cross-framework support
  - Impact: Can delay core Observer improvements
  - Mitigation: Prioritize the smallest useful integration slice first
