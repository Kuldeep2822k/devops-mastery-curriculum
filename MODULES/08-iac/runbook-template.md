---
title: Runbook Template (Module 08)
tags:
  - runbook
  - template
  - terraform
module: "08"
---

# Runbook Template — Module 08 Terraform

## Title

Terraform — <Symptom (plan surprises / apply failed / drift detected / lock stuck)>

## Impact

- What environment is affected?
- What change is blocked or risky?
- Severity:

## Safety and Preconditions

- No apply without plan review.
- Confirm correct environment/state before any changes.
- Treat state as sensitive; do not share in logs.

## Quick Triage (10 minutes)

```bash
pwd
terraform version
terraform workspace show || true
terraform validate
terraform plan
```

## Diagnosis

- Identify if drift exists (plan shows unexpected diffs).
- Identify replacements and why.
- Identify whether state is missing/corrupt.
- Identify provider/auth failures.

Useful commands:

```bash
terraform show
terraform state list
terraform state show <addr>
```

## Containment

- stop concurrent applies
- rollback config change (git revert) if plan is dangerous
- restore state from backup/version if state lost

## Fix

- drift reconciliation via apply
- state refactor via `terraform state mv`
- provider/version pinning fixes
- backend lock resolution (only with confirmation)

## Verification

- plan clean
- expected outputs stable (no secrets)
- post-apply smoke checks succeed (service-level)

## Prevention / Follow-Ups

- enforce plan review gate
- remote backend with locking and backups
- drift policy and audits
- ADR for backend/layout decisions
