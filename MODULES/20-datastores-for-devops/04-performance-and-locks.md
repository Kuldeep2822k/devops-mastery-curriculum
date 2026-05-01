---
title: 'Performance and Locks'
tags:
  - performance
  - postgres
module: "20"
---

# Performance and Locks

## Operator Mental Model

- Define what “healthy” looks like (signals and SLO-relevant symptoms).
- Identify the most common failure classes and what they imply.
- Use minimal commands to narrow blast radius and isolate layers.
- Prefer safe defaults and reversible changes during incidents.

## Evidence to Collect

- timestamps and scope (who/what/where)
- symptoms (errors, timeouts, saturation, user impact)
- one confirming metric/log/trace and one disconfirming signal

## Safe Moves

- reduce scope (canary, limit concurrency, isolate tenant)
- revert risky changes first; fix forward when you can prove it
- write down “next check” before running commands
