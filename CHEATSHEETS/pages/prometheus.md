---
title: 'Prometheus'
tags:
  - cheatsheet
  - prometheus
  - observability
---

# Prometheus Cheatsheet (Ops)

## Health

- `curl -fsS http://<prom>:9090/-/healthy`
- `curl -fsS http://<prom>:9090/-/ready`

## Targets + Scrape Issues

- `curl -fsS http://<prom>:9090/api/v1/targets | head -n 120`
- Look for: `lastError`, `lastScrape`, `health != up`.

## Query Debugging Tips

- Prefer aggregations that reduce cardinality:
  - `sum by (job) (...)`
- Beware label explosions in alerts:
  - Don’t alert per pod/container unless that’s intentional.

## Capacity Red Flags

- Disk usage / retention too high.
- High series count (cardinality) causing slow queries and instability.

