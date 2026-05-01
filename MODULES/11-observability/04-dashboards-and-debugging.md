---
title: Dashboards and Debugging (Dashboards as Products)
tags:
  - observability
  - dashboards
  - debugging
module: "11"
---

# Dashboards and Debugging (Dashboards as Products)

## Dashboards Answer Questions

A good dashboard is designed for an audience:

- on-call engineer needs “is it broken?” and “where?”
- service owner needs trend and regression visibility
- leadership needs SLO posture and risk

## Golden Signals (Common Baseline)

- traffic (throughput)
- errors (5xx, failures)
- latency (p95/p99)
- saturation (CPU/mem/queue depth)

## Drill-Down Paths

Dashboards should support:

- global → service → endpoint → dependency

Without drill-down, dashboards become screenshots.

## Debugging Workflow (Repeatable)

1) confirm user impact (SLO, errors, latency)  
2) identify which component changed (deploy/version)  
3) check saturation and dependency failures  
4) use traces to find slow span  
5) use logs to confirm root cause  
6) contain (rollback/traffic reduction) if needed  

## Anti-Patterns

- dashboards with too many panels and no narrative
- dashboards that require “tribal knowledge” to interpret
- missing version markers and deploy annotations
