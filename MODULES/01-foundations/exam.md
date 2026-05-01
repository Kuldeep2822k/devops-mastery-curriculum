---
title: Practical Exam
tags:
  - exam
  - foundations
module: "01"
---

# Practical Exam — Module 01 Foundations

## Rules

- Time-box: 60–90 minutes.
- No internet.
- Produce an exam submission in your evidence repo (commands + expected signals + reasoning).
- Do not print secrets.

## Tasks

### Task 1: Build a Minimal Service with Operational Interfaces

Requirements:

- A `/healthz` endpoint that returns 200 when healthy.
- Structured logs (at least JSON lines or consistent key/value lines).
- A runner script or Make target to start the service.

Grading criteria:

- health check works reliably (not just once)
- logs are high signal and consistent
- startup is repeatable (no manual hidden steps)

### Task 2: Failure Injection and Recovery

Requirements:

- Introduce a controlled failure that causes health to fail or latency to regress.
- Detect impact using repeated probes.
- Recover and verify the same probes return to baseline.

Grading criteria:

- you establish a baseline first
- you choose containment/rollback without thrashing
- you verify recovery with measurable signals

### Task 3: Write a Mini Runbook and an ADR

Requirements:

- A runbook entry: “Service health check failing” including diagnosis, containment, fix, verification.
- One ADR about a tradeoff you made (health definition, rollback trigger, logging format, etc).

Grading criteria:

- runbook steps are executable and include expected signals
- ADR includes constraints, options, decision, risks, mitigations

## Submission Checklist

- commands run (redacted)
- outputs/expected signals
- timeline of actions
- root cause and fix
- prevention step
