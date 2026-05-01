---
title: Practical Exam
tags:
  - exam
  - ci
module: "04"
---

# Practical Exam — Module 04 CI

## Rules

- Time-box: 120 minutes.
- No internet (except to view your own pipeline runs if using GitHub Actions).
- Submit evidence: commands + expected signals + reasoning.
- Do not print secrets.

## Tasks

### Task 1: Portable Pipeline Contract

Requirements:

- Provide `make lint`, `make test`, `make build`, `make package`, `make smoke`, `make ci`.
- `make smoke` must verify artifact existence and shape.

Grading criteria:

- targets are deterministic and idempotent
- artifact paths are consistent
- smoke gate catches missing dist/ errors

### Task 2: Clean Runner Reproduction

Requirements:

- run pipeline in a clean container runner and capture evidence of success.

Grading criteria:

- reproduction steps are copyable
- environment assumptions are explicit

### Task 3: GitHub Actions Implementation

Requirements:

- pipeline triggers on PR and push
- caching is enabled with safe keys
- dist artifacts uploaded
- fail-fast observed on intentional failure

Grading criteria:

- evidence includes run logs and artifact listing
- cache strategy is justified

### Task 4: CI Reliability Drill

Requirements:

- create a flaky failure (intentional nondeterminism) in a test or script
- diagnose it and fix it to be deterministic
- document a quarantine policy (even if you do not implement it)

Grading criteria:

- root cause is explained (not just “it was flaky”)
- fix makes signal stable
- prevention steps reduce recurrence

### Task 5: Runbook + ADR

Requirements:

- Runbook: “CI pipeline failing” including cache/artifact/lockfile diagnosis and verification.
- ADR: choose a pipeline design tradeoff (fail-fast policy, artifact layout, cache scope).
