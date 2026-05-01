---
title: "Deep Dive 01: Rollout Strategy Tradeoffs and Failure Domains"
tags:
  - delivery
  - deep-dive
  - tradeoffs
module: "07"
---

# Deep Dive 01 — Rollout Strategy Tradeoffs and Failure Domains

## Failure Domains You Must Name

Before choosing a strategy, name what can fail:

- single pod / node
- a release version
- a dependency (DB, cache, queue)
- traffic routing layer (ingress/proxy)
- data plane vs control plane

If you cannot name the failure domains, you will choose strategies based on aesthetics.

## Rolling vs Canary vs Blue/Green (Operational Lens)

Rolling update:

- failure domain: partial rollout; can affect many users before detection if signals are slow

Canary:

- failure domain: limited traffic subset
- requires telemetry and ability to control traffic

Blue/green:

- failure domain: “all traffic switch” (but reversible quickly)
- requires capacity headroom and session/state planning

## “Rollback Is Not a Time Machine”

Rollback does not undo:

- external side effects (emails sent, payments processed)
- data migrations
- cache poisoning

You must design rollback-safe changes, not just a rollback command.

## Staff-Level Standard

For high-risk systems:

- deploy is a controlled experiment
- define hypothesis (“this change should reduce latency by X”)
- define stop conditions and rollback triggers
- record outcomes and learnings
