---
title: '03-sre-readiness: Scenarios'
tags:
  - capstone
---

# 03-sre-readiness — Scenarios

For each scenario, capture:

- Symptoms
- Constraints
- Diagnosis commands
- Root cause
- Fix
- Verify
- Prevention

Create `evidence/timeline-<scenario>.md` per scenario.

## Scenario 01 — Alert storm

- Description: Multiple noisy alerts fire during a small incident.

### Expected Signals

- reduce pages
- keep coverage

### Tasks

- Triage
- Tune alerts
- Document policy

## Scenario 02 — Unknown unknowns

- Description: Dashboards are misleading; you must validate signals.

### Expected Signals

- cross-check logs/metrics
- identify gap

### Tasks

- Add instrumentation
- Write follow-up

## Scenario 03 — Error budget burn

- Description: Budget is burning; decide release freeze vs canary.

### Expected Signals

- explicit policy
- stakeholder comms

### Tasks

- Apply gate
- Run canary drill
- Write comms
