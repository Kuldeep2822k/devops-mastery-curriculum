---
title: Practical Exam
tags:
  - exam
  - observability
module: "11"
---

# Practical Exam — Module 11 Observability

## Rules

- Time-box: 120 minutes.
- No internet.
- Submit evidence: commands/queries + expected signals + reasoning.
- Do not print secrets.

## Tasks

### Task 1: Instrumentation

Requirements:

- define a structured log format and required fields
- define 3 key metrics (traffic, errors, latency)
- explain how you would correlate a slow request to logs/traces

Grading criteria:

- signal design is coherent and low-noise
- includes service/env/version fields

### Task 2: Alert Design

Requirements:

- design 2 actionable alerts:
  - availability
  - latency
- include runbook links and clear triage steps

Grading criteria:

- alerts map to user impact and have actionable content

### Task 3: SLO Definition

Requirements:

- define one SLI and one SLO for a service
- describe error budget policy implications (release vs toil decisions)

Grading criteria:

- SLO is measurable and aligned to user value

### Task 4: Runbook + ADR

Requirements:

- Runbook: “high error rate / high latency” with first 10 minute steps.
- ADR: decide an alerting policy (burn-rate, paging rules) or SLO definition and justify tradeoffs.
