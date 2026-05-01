---
title: "Deep Dive 01: Resource Management Tradeoffs (Requests, Limits, and QoS)"
tags:
  - kubernetes
  - deep-dive
  - resources
module: "06"
---

# Deep Dive 01 — Resource Management Tradeoffs (Requests, Limits, and QoS)

## Requests Drive Scheduling Reality

If requests are too low:

- scheduler packs too many pods
- node becomes saturated
- latency rises and failures cascade

If requests are too high:

- pods stay Pending
- capacity is underutilized

Staff-level habit:

- choose initial requests based on measurements
- adjust iteratively using observed usage

## Limits: Safety vs Performance

Memory limits:

- exceeding limit triggers OOMKill
- OOMKilled pods can look like “random crash loops”

CPU limits:

- exceeding CPU limit triggers throttling
- throttling creates latency and timeouts without obvious errors

Guidance:

- always set memory limits for safety
- set CPU limits carefully; consider leaving CPU unlimited for latency-sensitive services (context-dependent) while setting requests

## QoS Classes (Conceptual)

QoS depends on requests/limits configuration.

Operational meaning:

- under node pressure, some pods are evicted first

If you don’t understand QoS, eviction behavior will look random during incidents.

## Anti-Patterns

- copying “standard” limits without measuring
- setting tiny limits to “save cost” and causing constant OOMs
- using restarts as a substitute for proper resource policy

## What to Document in an ADR

- how you choose requests/limits
- which workloads are allowed to burst CPU
- what signals trigger tuning or scaling
