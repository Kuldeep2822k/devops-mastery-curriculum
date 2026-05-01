---
title: 'Grafana'
tags:
  - cheatsheet
  - grafana
  - observability
---

# Grafana Cheatsheet (Dashboard Debugging)

## First Questions

- Is the system broken, or is the dashboard broken?
- Does the panel query match the intended labels and time window?

## Common Pitfalls

- Wrong time range or timezone assumptions.
- Aggregation hides partial failures (e.g., `avg` hides one bad region).
- Using `irate`/`rate` windows that are too short/too long.
- Mixing units (ms vs s) or percent vs ratio.

## Debug Workflow

- Copy the panel query into Explore.
- Add breakdowns:
  - by region/cluster/pod
- Add annotations for deploys and incidents.

