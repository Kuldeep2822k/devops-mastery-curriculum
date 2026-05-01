---
title: Runbook Template (Module 01)
tags:
  - runbook
  - template
  - foundations
module: "01"
---

# Runbook Template — Module 01 Foundations

## Title

<Service> — <Symptom>

## Impact

- User impact:
- Scope:
- Severity:

## Preconditions / Safety

- Are you in the correct environment?
- What actions are disallowed (e.g., no reboot, one restart budget)?
- What evidence must be captured before changes?

## Quick Triage (2–5 minutes)

- Health check:
  - Command:
  - Expected:
- Port/process check:
  - Command:
  - Expected:

## Diagnosis

### Hypotheses (Write 2–3)

- H1:
- H2:
- H3:

### Commands and Expected Signals

- Command:
  - Why:
  - Expected signal:
  - Interpretation:

## Containment

Choose the least risky option:

- rollback to last known good
- reduce load / disable a feature
- isolate a bad instance

Steps:

1.

Verification:

- Command:
- Expected:

## Fix

Minimal change steps:

1.

Verification:

- Command:
- Expected:

## Post-Fix Monitoring (Time Window)

- What signals to watch:
- How long:
- What triggers re-escalation:

## Prevention / Follow-Ups

- Add guardrail:
- Improve signal:
- Improve runbook:
