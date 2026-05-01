---
title: 'Deep Dive 02'
tags:
  - deep-dive
  - artifacts
  - sbom
  - provenance
module: "17"
---

# Deep Dive 02

## Tradeoffs

- What you gain (speed, safety, simplicity) vs what you pay (latency, complexity, toil).
- Which failure modes become easier vs harder to diagnose.

## Edge Cases

- partial rollouts and mixed versions
- time skew and expiry-driven failures
- retries amplifying load (retry storms)

## Anti-Patterns

- “disable verification” as a default fix
- unbounded retries and unbounded fan-out
- hidden coupling (shared state without contracts)
