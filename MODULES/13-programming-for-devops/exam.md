---
title: Practical Exam
tags:
  - exam
  - programming
module: "13"
---

# Practical Exam — Module 13 Programming for DevOps

## Rules

- Time-box: 120 minutes.
- No internet.
- Submit evidence: commands + outputs (redacted) + reasoning.

## Tasks

### Task 1: Safe CLI Tool

Requirements:

- implement dry-run default
- implement `--apply` for destructive actions
- implement max limit and clear exit codes

Grading criteria:

- bounded and reversible behavior
- safe by default

### Task 2: Resilient API Client

Requirements:

- timeouts
- retries with backoff
- rate limit handling
- pagination with max bound

Grading criteria:

- does not cause retry storms
- handles transient failures safely

### Task 3: Runbook + ADR

Requirements:

- Runbook: “automation tool caused incident / runaway script” with containment and verification steps.
- ADR: choose automation safety standards (limits, approvals, dry-run requirements).
