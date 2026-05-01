---
title: Rubric (0–4)
tags:
  - rubric
  - linux
module: "02"
---

# Rubric — Module 02 Linux

Use global rubric meanings: [Evidence and Rubrics](../../00-HOW-TO-USE/04-evidence-rubrics.md)

## Skill 1: Linux Triage (CPU/Mem/Disk/Network)

- 0: Cannot triage without step-by-step guidance; misses obvious signals.
- 1: Runs commands but cannot interpret; conflates symptoms.
- 2: Can identify resource pressure and top offenders; distinguishes refused vs timeout.
- 3: Can form hypotheses and prioritize actions; preserves evidence before restarts.
- 4: Can design guardrails and monitoring; teaches others and improves runbooks.

## Skill 2: systemd/journald Operations

- 0: Restarts blindly; cannot interpret unit status or exit reasons.
- 1: Can view status and logs but struggles to connect to root cause.
- 2: Can debug common unit failures (bad path, env var, permissions) and verify.
- 3: Can tune restart behavior and timeouts safely with rollback plan and evidence.
- 4: Designs operationally robust unit patterns; reduces MTTR with strong docs.

## Skill 3: Permissions and Filesystems

- 0: Uses unsafe permission changes as default.
- 1: Can read modes but not reason about directory traversal and service users.
- 2: Can diagnose and fix permission issues safely; uses least privilege.
- 3: Anticipates failure modes (log dirs, data dirs, low ports) and documents them.
- 4: Designs hardened layouts and guardrails without breaking operability.

## Skill 4: Incident Practice and Writing

- 0: No timeline, no verification, unclear actions.
- 1: Some notes but missing expected signals and prevention.
- 2: Clear diagnosis/fix/verification logs; runbook is usable.
- 3: Good containment, minimal changes, explicit tradeoffs in ADR.
- 4: Produces high-quality runbooks and prevention tasks that reduce recurrence.
