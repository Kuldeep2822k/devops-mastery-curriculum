---
title: "Deep Dive 02: On-Call Sustainability and Alert Economics"
tags:
  - sre
  - deep-dive
  - oncall
module: "12"
---

# Deep Dive 02 — On-Call Sustainability and Alert Economics

## Pager Load Is a Reliability Metric

If on-call load is high:

- engineers burn out
- alerts get ignored
- incidents last longer

Treat pager load like a system metric:

- pages per shift
- after-hours pages
- top noisy alerts

## Alert Economics

Every alert has a cost:

- interruption cost
- context switching cost
- increased error rate under stress

So:

- page only on actionable user impact
- convert noisy alerts to tickets or dashboards

## Staff-Level Standard

- define paging policy in an ADR
- review alert noise monthly
- invest in runbooks and automation to reduce MTTR
