---
title: Deployment Strategies (Rolling, Blue/Green, Canary)
tags:
  - delivery
  - deployments
  - rollbacks
module: "07"
---

# Deployment Strategies (Rolling, Blue/Green, Canary)

## Rolling Update

Mechanism:

- replace pods gradually within the same Deployment

Pros:

- simple
- default in Kubernetes

Cons:

- rollback may still be slow if issue is data/schema-related
- partial exposure can still cause user impact during rollout

Best for:

- low/medium risk changes with strong readiness checks

## Blue/Green

Mechanism:

- run two environments (blue and green)
- switch traffic quickly

Pros:

- fast rollback (switch back)
- clear separation between versions

Cons:

- doubles resources
- requires careful handling of sessions/state

Best for:

- high-risk changes
- when fast rollback is critical

## Canary

Mechanism:

- send a small % of traffic to new version
- watch signals, then increase

Pros:

- reduces blast radius
- allows detection of subtle regressions (latency, error rate)

Cons:

- requires good telemetry and traffic control
- can mask issues that only occur at scale

Best for:

- changes with uncertain risk
- performance-sensitive systems

## Choosing Strategy: A Simple Decision Lens

- Can you rollback safely? (schema and config compatible)
- Do you have fast signals? (dashboards, alerting, smoke checks)
- What is acceptable blast radius?
- What is your capacity headroom?

Staff-level practice:

- write the choice in an ADR and include rollback triggers.
