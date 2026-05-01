---
title: Rubric (0–4)
tags:
  - rubric
  - ansible
module: "09"
---

# Rubric — Module 09 Config Mgmt (Ansible)

Use global rubric meanings: [Evidence and Rubrics](../../00-HOW-TO-USE/04-evidence-rubrics.md)

## Skill 1: Idempotent Automation

- 0: Non-idempotent scripts; repeated runs cause changes.
- 1: Mixed idempotency; relies on shell and fragile checks.
- 2: Uses modules/templates; second run is clean.
- 3: Builds safe handler patterns and consistent diff behavior.
- 4: Creates reusable automation standards and reduces fleet incidents.

## Skill 2: Inventory and Variables Design

- 0: Hardcoded hosts and values.
- 1: Inventory exists but variable sources are confusing.
- 2: Clean group_vars/host_vars structure with clear defaults.
- 3: Variable precedence is understood and controlled; secrets managed safely.
- 4: Designs scalable inventory for large fleets and multi-env setups.

## Skill 3: Operational Safety (Blast Radius Control)

- 0: Runs against all hosts with no safety gates.
- 1: Uses limits sometimes but inconsistent.
- 2: Uses `--limit`, serial, and check mode as standard.
- 3: Canary rollouts and stop conditions are explicit; rollback is documented.
- 4: Builds org-wide guardrails and audit-friendly workflows.

## Skill 4: Troubleshooting and Writing

- 0: Cannot debug playbook failures.
- 1: Uses verbose output but struggles to isolate root cause.
- 2: Evidence-based debugging and a usable runbook.
- 3: Strong runbooks and ADRs that reduce MTTR.
- 4: Teaches others and builds scalable operational docs.
