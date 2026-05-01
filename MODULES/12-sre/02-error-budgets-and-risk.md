---
title: Error Budgets and Risk Management
tags:
  - sre
  - slo
  - error-budgets
module: "12"
---

# Error Budgets and Risk Management

## Error Budget as Decision Tool

Error budget is the “allowed unreliability” for an SLO window.

Use it to decide:

- can we ship risky changes now?
- should we focus on reliability work?
- when to freeze releases

## Policy Examples

If error budget healthy:

- normal release velocity allowed
- invest in feature work

If error budget burning fast:

- tighten release gates
- focus on reliability and incident reduction

If error budget exhausted:

- freeze risky releases
- prioritize reliability work and toil reduction

## Risk Management Practices

- canary for higher-risk changes
- smaller diffs
- explicit rollback triggers and windows
- feature flags to decouple deploy and behavior

## Anti-Patterns

- SLO exists but nobody uses it for decisions
- error budget used as punishment instead of learning tool
- “always ship” regardless of reliability posture
