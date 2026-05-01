---
title: Incident Management (Roles, Timeline, Comms)
tags:
  - sre
  - incident
module: "12"
---

# Incident Management (Roles, Timeline, Comms)

## Incident Response Goals

- reduce user impact quickly
- avoid making things worse
- preserve evidence for root cause analysis
- coordinate people and decisions cleanly

## Roles (Minimal Set)

- Incident Commander (IC): owns coordination and decisions
- Operations/Responder: executes technical actions
- Communications: updates stakeholders

In small teams, one person may hold multiple roles, but responsibilities must still be explicit.

## Timeline Discipline

Write down:

- timestamps
- actions taken
- evidence observed
- decisions and why

This prevents “we forgot what we changed” during long incidents.

## Comms Hygiene

Good updates include:

- impact summary
- current status
- what we are doing next
- next update time

Avoid:

- speculation
- blame

## Containment vs Fix

Containment:

- rollback
- disable feature flag
- reduce load

Fix:

- code change
- config change

Default under high severity:

- contain first, then fix with a controlled change.
