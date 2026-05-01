---
title: '01-local-platform: Scenarios'
tags:
  - capstone
---

# 01-local-platform — Scenarios

For each scenario, capture:

- Symptoms
- Constraints
- Diagnosis commands
- Root cause
- Fix
- Verify
- Prevention

Create `evidence/timeline-<scenario>.md` per scenario.

## Scenario 01 — Golden path adoption failure

- Description: A team tries the template but CI fails and artifacts are missing.

### Expected Signals

- make ci
- dist/ exists
- runbook updated

### Tasks

- Fix template contract
- Add verify signals
- Update docs

## Scenario 02 — Rollback under pressure

- Description: A bad change is promoted; you must rollback and verify stability.

### Expected Signals

- explicit rollback procedure
- verification window

### Tasks

- Rollback
- Verify
- Write postmortem

## Scenario 03 — Guardrail false positive

- Description: Policy gate blocks a legitimate change; you must tune it without weakening safety.

### Expected Signals

- reduce false positives
- maintain security intent

### Tasks

- Add allowlist
- Add tests
- Document rationale
