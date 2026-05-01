---
title: SLOs, SLIs, and Error Budgets
tags:
  - observability
  - slo
  - sli
module: "11"
---

# SLOs, SLIs, and Error Budgets

## Definitions

- SLI: what you measure (success rate, latency, freshness)
- SLO: target for the SLI over a window (e.g., 99.9% over 30d)
- error budget: allowed amount of “badness” in that window

## Why SLOs Matter for Operators

SLOs provide:

- a shared language for reliability
- a way to prioritize work (toil vs features)
- a basis for alerting (burn rate)

## Good SLIs (Practical)

User-centric examples:

- availability: successful requests / total
- latency: % of requests under threshold
- correctness: critical job completed on time

Avoid SLIs that are too internal:

- CPU usage as an SLI

## Alerting With Error Budgets

Alert on burn rate:

- fast burn (e.g., will consume budget in hours)
- slow burn (e.g., will consume budget in days)

The goal:

- page only when reliability is actually threatened.

## Anti-Patterns

- SLOs with no measurement path
- SLOs set without stakeholder agreement
- alerts that don’t map to SLO impact
