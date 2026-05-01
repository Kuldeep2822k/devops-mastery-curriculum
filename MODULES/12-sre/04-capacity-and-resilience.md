---
title: Capacity and Resilience (Practical)
tags:
  - sre
  - capacity
  - resilience
module: "12"
---

# Capacity and Resilience (Practical)

## Capacity Planning Basics

You need:

- traffic model (RPS, concurrency, batch sizes)
- resource model (CPU/mem/IO/network)
- headroom policy (how much spare capacity you keep)

The goal is not perfect prediction. The goal is avoiding predictable outages.

## Common Failure Patterns

- no headroom → deploy or traffic spike causes outage
- CPU throttling → latency spikes without clear errors
- queue growth → slow burn incidents
- dependency saturation → retry storms

## Resilience Practices

- timeouts and retries with backoff
- circuit breakers / bulkheads
- load shedding / graceful degradation
- chaos drills (controlled)

## Verification Mindset

After changes:

- verify with real user-like checks
- watch the right window (don’t declare victory instantly)

## Anti-Patterns

- scaling as the only response (without understanding the bottleneck)
- retries without backoff
- no load tests and no capacity posture
