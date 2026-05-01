---
title: Practical Exam
tags:
  - exam
  - security
module: "10"
---

# Practical Exam — Module 10 Security

## Rules

- Time-box: 120 minutes.
- No internet.
- Submit evidence: commands + expected signals + reasoning.
- Do not print secrets.

## Tasks

### Task 1: Threat Model a Small Service

Requirements:

- list assets, entry points, trust boundaries
- list top 5 threats and corresponding controls and detection signals

Grading criteria:

- practical controls and clear detection signals

### Task 2: Secrets Guardrail

Requirements:

- implement a pre-commit or CI-style secret scan gate
- demonstrate it blocks a secret-like pattern

Grading criteria:

- blocks commit/pipeline reliably with clear message

### Task 3: Supply Chain Baseline Gate

Requirements:

- enforce a policy like “no :latest base images”
- generate and store a dependency manifest (requirements, lockfile, etc.)

Grading criteria:

- gate fails on violation and passes when fixed

### Task 4: Runbook + ADR

Requirements:

- Runbook: “Secret leaked” including containment, rotation, and verification steps.
- ADR: decide which security gates are mandatory in CI and justify tradeoffs.
