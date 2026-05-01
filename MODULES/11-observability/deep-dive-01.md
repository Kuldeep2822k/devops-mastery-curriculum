---
title: "Deep Dive 01: Cardinality, Cost, and Data Quality"
tags:
  - observability
  - deep-dive
  - metrics
module: "11"
---

# Deep Dive 01 — Cardinality, Cost, and Data Quality

## Cardinality Is the Hidden Cost Driver

High-cardinality dimensions (user_id, request_id) explode:

- metrics storage
- query cost
- dashboard performance

Guidance:

- use low-cardinality labels (endpoint, status_class, region)
- keep request IDs in logs/traces, not metrics labels

## Data Quality Pitfalls

Common issues:

- missing tags (env/service/version)
- inconsistent units (ms vs seconds)
- counters that reset and confuse alert logic

## Operator Habit

Treat observability as a product:

- version the schema (field names and meanings)
- document key dashboards and alerts
- review alert noise and dashboard correctness periodically
