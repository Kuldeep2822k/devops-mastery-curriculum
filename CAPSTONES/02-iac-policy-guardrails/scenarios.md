---
title: '02-iac-policy-guardrails: Scenarios'
tags:
  - capstone
---

# 02-iac-policy-guardrails — Scenarios

For each scenario, capture:

- Symptoms
- Constraints
- Diagnosis commands
- Root cause
- Fix
- Verify
- Prevention

Create `evidence/timeline-<scenario>.md` per scenario.

## Scenario 01 — Destructive plan detected

- Description: Plan shows deletes/replacements unexpectedly.

### Expected Signals

- identify why
- safe mitigation

### Tasks

- Add preflight gate
- Document exceptions

## Scenario 02 — Drift vs intent

- Description: Infra drift exists but must be reconciled without outage.

### Expected Signals

- detect drift
- reconcile safely

### Tasks

- Apply safe changes
- Update runbook

## Scenario 03 — Break-glass override

- Description: Urgent fix needs override; enforce verification and audit.

### Expected Signals

- audit trail
- verification window

### Tasks

- Override
- Verify
- Record ADR
