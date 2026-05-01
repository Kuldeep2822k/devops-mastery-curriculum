---
title: "Deep Dive 01: Resource Pressure, Saturation, and Cascading Failures"
tags:
  - linux
  - deep-dive
  - performance
module: "02"
---

# Deep Dive 01 — Resource Pressure, Saturation, and Cascading Failures

## Why “The Host” Still Matters

Even in container and Kubernetes worlds, host behavior drives outages:

- CPU saturation increases latency across everything
- memory pressure triggers OOM kills and kernel reclaim storms
- disk pressure breaks logging, databases, and package managers
- network issues cause retry storms and thundering herds

## CPU Saturation (Practical)

Signals:

- high load average (`uptime`)
- high %CPU for one or more processes (`top`, `ps`)
- latency increases but error rate may stay low initially

Common operator mistakes:

- scaling the service without understanding the bottleneck
- restarting, which increases load due to cold caches

## Memory Pressure and OOM

Signals:

- high RSS memory
- swap activity (if enabled)
- OOM kills in logs (kernel messages)

Diagnosis hints:

- memory issues can look like random crashes
- look for “killed process” events and abrupt exits

## Disk Pressure

Disk pressure causes weird behavior:

- services crash writing logs
- databases fail fsync
- system upgrades fail

Signals:

- `df -h` shows high usage
- journald may drop logs

## Cascading Failure Pattern: Retries + Timeouts

Common chain:

1) dependency slows (CPU, disk, network)
2) clients retry aggressively
3) traffic multiplies
4) more saturation
5) wider outage

Operator countermeasures:

- rate limiting
- timeouts tuned for safety
- circuit breakers / fail-open decisions (context-dependent)
- rollback to reduce load or revert regression

## Staff-Level Habit: Capacity Is a Design Input

Treat capacity like a requirement:

- expected request rate
- concurrency
- connection limits
- disk growth

Document assumptions in ADRs and verify them with load/failure tests over time.
