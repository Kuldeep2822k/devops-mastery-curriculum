---
title: Practical Exam
tags:
  - exam
  - delivery
module: "07"
---

# Practical Exam — Module 07 Delivery

## Rules

- Time-box: 120 minutes.
- No internet.
- Submit evidence: commands + expected signals + reasoning.

## Tasks

### Task 1: Rollout + Rollback Drill

Requirements:

- Deploy a workload to Kubernetes with readiness checks.
- Introduce a controlled failure (probe/labels/config).
- Detect impact via endpoints/events and user-facing checks.
- Roll back and verify recovery.

Grading criteria:

- evidence-first debugging
- rollback executed safely and verified over a window
- artifact identity and change recorded

### Task 2: Canary Evaluation and Promotion

Requirements:

- Run stable and canary concurrently.
- Demonstrate how you evaluate canary health (signals).
- Promote canary or rollback and justify the decision.

Grading criteria:

- explicit stop conditions and decision points
- correct Kubernetes mechanics and verification

### Task 3: Runbook + ADR

Requirements:

- Runbook: “Bad deploy causing Service 503 / latency spike” with containment and rollback steps.
- ADR: choose a deployment strategy for a service and justify tradeoffs and migration constraints.

Grading criteria:

- runbook is executable and includes expected signals
- ADR includes constraints, options, decision, risks, mitigations
