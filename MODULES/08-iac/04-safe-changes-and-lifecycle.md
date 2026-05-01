---
title: Safe Changes and Lifecycle Control
tags:
  - terraform
  - safety
  - lifecycle
module: "08"
---

# Safe Changes and Lifecycle Control

## Plan Review Is a Safety Gate

Before apply, you should know:

- what will be created/updated/destroyed
- what will be replaced (forces replacement)
- whether any destructive change is acceptable

Operational habit:

- treat plan output as the diff you must approve
- keep changes small and reversible

## Lifecycle Meta-Arguments (Use Carefully)

Common tools:

- `prevent_destroy`: blocks destroy for critical resources
- `create_before_destroy`: reduces downtime on replacements
- `ignore_changes`: prevents Terraform from reconciling certain fields

Tradeoffs:

- these controls can hide drift or make changes harder later

## Safe Rollout Patterns

- separate risky changes into multiple applies
- for production, require:
  - peer review of plan
  - change record (who/why)
  - verification checklist

## Verification After Apply

Terraform success means:

- API calls succeeded

It does not guarantee:

- the service is healthy
- dependencies are reachable

Staff-level behavior:

- pair IaC apply with health checks and smoke tests.

## Anti-Patterns

- applying large diffs without understanding impact
- using ignore_changes to silence real drift
- removing state to “fix” errors without understanding resource ownership
