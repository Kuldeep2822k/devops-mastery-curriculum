---
title: Reliability Basics (SLO Thinking)
tags:
  - foundations
  - reliability
  - slo
module: "01"
---

# Reliability Basics (SLO Thinking)

## Reliability Is a Product Attribute

Users experience reliability, not your architecture:

- Does it respond?
- Is it fast enough?
- Does it fail gracefully?
- Do errors recover quickly?

Operationally: reliability is outcomes over time, not a single successful test.

## The Three Core Signals (RED)

For request-driven systems:

- Rate: volume of requests
- Errors: failure rate (5xx, timeouts, failed jobs)
- Duration: latency (p50/p95/p99)

For resource-driven systems:

- utilization and saturation (CPU, memory, disk, IO, connection pools)

## SLOs at a Beginner Level

An SLO is a target for a user-facing behavior.

Example:

- “99.9% of requests return 2xx within 300ms over 7 days”

Key ideas:

- choose one or two SLOs per service, not 20
- define what counts as “good” vs “bad”
- measure it from the user perspective where possible

## Error Budgets (Intuition)

Error budget is the allowed unreliability.

Why operators care:

- it defines how aggressive you can be with changes
- it creates a structured argument against “ship everything now”

## Reliability vs Availability vs Durability

- availability: is it up right now
- reliability: does it meet expectations over time
- durability: is the data safe even if systems fail

Operators must know which dimension is actually being discussed.

## Anti-Patterns

- measuring only “up/down” without latency or error rates
- dashboards that are not tied to user behaviors
- alerts that trigger on symptoms without context or runbooks

This module builds the habit of defining signals and verifying changes using signals, not vibes.
