---
title: Rubric (0–4)
tags:
  - rubric
  - ci
module: "04"
---

# Rubric — Module 04 CI

Use global rubric meanings: [Evidence and Rubrics](../../00-HOW-TO-USE/04-evidence-rubrics.md)

## Skill 1: Pipeline Architecture and Artifact Contracts

- 0: Treats CI as a single script; no artifacts or stages.
- 1: Has stages but weak boundaries; artifacts rebuilt or inconsistent.
- 2: Defines clear stages and artifact contracts; can reason about dependencies.
- 3: Designs for speed and reliability (parallelism, caching correctness, fail-fast).
- 4: Designs pipelines as products with SLOs and clear ownership.

## Skill 2: Secrets and Environment Separation

- 0: Secrets in code/logs; no separation.
- 1: Uses secret store but exposes too broadly; unclear env boundaries.
- 2: Secrets injected only where needed; PRs do not access prod secrets.
- 3: Uses approvals/protected environments; least privilege tokens and auditing mindset.
- 4: CI threat model is explicit; supply-chain controls integrated and enforced.

## Skill 3: CI Reliability Engineering

- 0: Flaky tests ignored; reruns without tracking.
- 1: Can identify flakes but no systematic mitigation.
- 2: Can apply timeouts and stabilize tests; tracks failures.
- 3: Builds quarantine policy, flake dashboards, and reduces noise.
- 4: Drives CI SLO improvements and reduces delivery outages org-wide.

## Skill 4: Troubleshooting and Operability

- 0: Cannot debug CI failures; relies on guesswork.
- 1: Can read logs but struggles to isolate cause (cache/artifact/env).
- 2: Uses structured diagnosis and verifies fixes; can reproduce locally.
- 3: Writes strong runbooks and builds tooling to reduce MTTR.
- 4: Prevents classes of failures through guardrails and policy.
