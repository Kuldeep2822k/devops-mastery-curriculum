---
title: State, Locking, and Drift
tags:
  - terraform
  - state
  - drift
module: "08"
---

# State, Locking, and Drift

## State Is Source of Operational Truth (for Terraform)

State tracks:

- what Terraform believes exists
- resource IDs and attributes

State often contains sensitive data. Treat it like a secret-bearing asset.

## Locking Prevents Corruption

Without locking:

- two applies can write conflicting state updates
- you get broken state and unpredictable infra

Operational rule:

- only one apply at a time per state.

Local state has limited locking. Remote backends add real locking (recommended in teams).

## Drift

Drift means:

- real system changed outside Terraform

Drift causes:

- plan surprises
- apply failures

How you detect drift:

- `terraform plan` shows unexpected diffs
- `terraform refresh` (or plan with refresh) updates state

Staff-level habit:

- treat drift as a process failure (manual changes, broken automation), not as “Terraform weirdness”.

## State Recovery (Core Moves)

- back up state regularly (remote backends do this better)
- use `terraform state list/show/rm/mv` with discipline
- avoid “delete state and reapply” unless you fully understand consequences

## Anti-Patterns

- manual changes in production without recording them
- sharing state files over chat or email
- “force unlock” without confirming no other apply is running
