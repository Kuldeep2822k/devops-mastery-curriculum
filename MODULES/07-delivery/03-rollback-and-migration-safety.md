---
title: Rollbacks and Migration Safety (The Hard Part)
tags:
  - delivery
  - rollback
  - migrations
module: "07"
---

# Rollbacks and Migration Safety (The Hard Part)

## Why Rollbacks Fail in Real Life

Rollback failure is usually not a tooling issue. It is usually one of:

- schema incompatibility (old app can’t read new schema)
- config incompatibility (old app expects config keys removed/renamed)
- data migrations are destructive or irreversible
- dependency versions changed (e.g., cache format, queue contracts)

If rollback is not safe, your “deployment strategy” is mostly theater.

## Backward-Compatible Changes (Practical)

Database and API safety patterns:

- expand/contract migrations (add columns first, then use, then remove later)
- feature flags to decouple deploy from behavior change
- dual-write / dual-read where necessary (with careful monitoring)

## Rollback Triggers Must Be Explicit

Define triggers:

- error rate threshold over a time window
- latency p95 threshold
- critical user flow failing
- health/readiness failures

Define how to rollback:

- to which artifact identity (digest/tag)
- who can execute rollback
- what verification is required after rollback

## “Rollback to Previous Version” Is Not Enough

You must also consider:

- config rollback (ConfigMaps, flags)
- traffic routing rollback (ingress/service selectors)
- background jobs and workers that may still be running old/new logic

## Anti-Patterns

- running migrations “because deploy succeeded”
- deleting columns immediately (no rollback path)
- changing multiple contracts at once (API + DB + queue)
