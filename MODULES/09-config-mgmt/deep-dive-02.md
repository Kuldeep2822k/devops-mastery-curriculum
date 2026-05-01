---
title: "Deep Dive 02: Fleet Rollouts (Serial, Batches, and Safety Gates)"
tags:
  - ansible
  - deep-dive
  - rollouts
module: "09"
---

# Deep Dive 02 — Fleet Rollouts (Serial, Batches, and Safety Gates)

## Why Fleet Changes Are Different

One mistake can break hundreds of machines fast.

So you need:

- batching
- health verification
- stop conditions

## Patterns

### Serial

Run on one host at a time:

- lowest risk
- slowest

### Batches

Run on N hosts at a time:

- balances speed and safety
- requires good verification

### Canary Group

Always apply to a small “canary” group first:

- verify signals
- then expand to rest of fleet

## Stop Conditions

Stop when:

- error rate above threshold
- service health checks fail
- unexpected config diffs appear

## Operational Writing

Runbooks should include:

- how to run with `--limit`
- how to run in check mode
- how to rollback to previous templates
