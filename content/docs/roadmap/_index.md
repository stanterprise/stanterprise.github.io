---
title: Roadmap
weight: 6
description: Current Observer roadmap themes and target windows
---

# Observer Roadmap

This page captures the current product direction for Observer. Targets are approximate and may shift as implementation details firm up.

## Status Legend

- Planned
- In Progress
- Blocked
- Released

## Now

| Item           | Target  | Status  | Notes                                                                                             |
| -------------- | ------- | ------- | ------------------------------------------------------------------------------------------------- |
| Pytest support | Q3 2026 | Planned | Prioritize parity with the existing Playwright reporter flow before expanding framework coverage. |

## Next

| Item                       | Target  | Status  | Notes                                                                                  |
| -------------------------- | ------- | ------- | -------------------------------------------------------------------------------------- |
| MCP Server                 | Q3 2026 | Planned | Focus on a minimal, stable interface that can expose Observer data and actions safely. |
| Manager View               | Q4 2026 | Planned | Tailor the experience for higher-level run summaries and team-level reporting.         |
| JUnit 5 support            | Q4 2026 | Planned | Align the reporter model with JUnit 5 lifecycle events and metadata.                   |
| GitHub Actions integration | Q4 2026 | Planned | Keep setup lightweight so it is easy to adopt in existing CI workflows.                |

## Later

| Item                      | Notes                                   |
| ------------------------- | --------------------------------------- |
| Executive Summary View    | High-level reporting for stakeholders   |
| OpenTelemetry integration | Future observability export path        |
| JUnit XML importer        | Broader test result ingestion support   |
| .NET ecosystem            | Potential new reporter surface          |
| Prometheus / Grafana      | Metrics export and dashboarding         |
| Datadog integration       | Third-party observability compatibility |
| Slack integration         | Alerting and notifications              |
| Cypress                   | Additional test framework support       |

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
