---
title: Design Partners
description: Help validate Observer's test observability workflows with real Playwright suites.
body_class: design-partners-page
---

# Design Partners

Observer is an open-source test observability project for following automated test execution and investigating runs, failures, attempts, steps, and attachments. We are looking for engineering teams to try it in realistic workflows and help shape what should come next.

## Current Stage

Observer is evolving and seeking early real-world validation. The currently available reporter integration is Playwright. Pytest and Mocha reporter work is in progress. This program is for product feedback and workflow validation; it is not a claim of production maturity or existing customer adoption.

## Who Should Take Part

Design partners may be a good fit if they:

- maintain a meaningful Playwright suite or CI regression workflow;
- investigate flaky tests or failures across retries and steps;
- run tests in parallel or across shards;
- own test infrastructure, developer tooling, SDET, platform, or quality workflows;
- want to explore test observability, even if their setup does not match every example.

## What Participation Involves

- Try the hosted demo or deploy Observer in a self-hosted environment.
- Connect a Playwright suite when practical and observe a representative run.
- Share feedback about setup, terminology, failure investigation, missing information, and usability.
- Optionally join a short conversation to discuss workflows and findings.

Participation is collaborative and scoped to what each team can reasonably evaluate; there is no required support commitment or fixed meeting cadence.

## What Participants Can Expect

- Direct input into product priorities and workflows.
- A conversation with the maintainer and help with integration where feasible.
- An open-source, self-hostable project under the Apache 2.0 license.
- Early discussion of relevant features as they develop, without a promise of delivery dates.

## What We're Trying to Learn

- Whether the current event model captures the signals engineers need.
- What information is useful while a run is still executing.
- How teams move from a failed result to a useful investigation.
- Which cross-run comparisons help identify changes in test behavior.
- Where installation and reporter setup create friction.
- What platform and quality teams need beyond individual test results.

## Project and Evaluation Links

- [Try the hosted demo](/docs/demo/)
- [Getting Started](/docs/getting-started/)
- [Apache 2.0 source repository](https://github.com/stanterprise/observer)
- [Playwright reporter on npm](https://www.npmjs.com/package/@stanterprise/playwright-reporter)
- Docker images: `ghcr.io/stanterprise/observer`
- Helm chart: `oci://ghcr.io/stanterprise/observer/charts/observer`

## Start a Conversation

Use [Observer GitHub Discussions](https://github.com/stanterprise/observer/discussions) to introduce your team, describe your test workflow, and suggest what you would like to evaluate. Please avoid posting private test data or secrets.