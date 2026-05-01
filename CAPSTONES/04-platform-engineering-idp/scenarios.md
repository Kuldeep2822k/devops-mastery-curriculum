---
title: '04-platform-engineering-idp: Scenarios'
tags:
  - capstone
---

# 04-platform-engineering-idp — Scenarios

For each scenario, capture:

- Symptoms
- Constraints
- Diagnosis commands
- Root cause
- Fix
- Verify
- Prevention

Create `evidence/timeline-<scenario>.md` per scenario.

## Scenario 01 — New service onboarding

- Description: A new service must be created via golden path in <30 minutes.

### Expected Signals

- scaffold
- ci passes
- docs exist

### Tasks

- Use template
- Verify
- Capture evidence

## Scenario 02 — Multi-tenant access failure

- Description: A team cannot deploy due to RBAC misconfig.

### Expected Signals

- can-i diagnosis
- least privilege fix

### Tasks

- Fix RBAC
- Verify
- Update runbook

## Scenario 03 — Supply chain gate

- Description: Dependency update fails SBOM/policy checks.

### Expected Signals

- determine root cause
- safe update

### Tasks

- Fix deps
- Re-run gate
- Document tradeoff
