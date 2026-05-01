---
title: Common Mistakes (Wrong vs Right)
tags:
  - mistakes
  - observability
module: "11"
---

# Common Mistakes — Module 11 Observability

## 1) Wrong: Alert on Everything

Wrong pattern:

- page on CPU spikes, single host blips, and every error log

Right pattern:

- page on user impact and SLO threats

## 2) Wrong: No Runbook Link

Wrong pattern:

- alert fires with no guidance

Right pattern:

- alert includes first commands and runbook link

## 3) Wrong: High-Cardinality Metrics Labels

Wrong pattern:

- user_id/request_id in metrics labels

Right pattern:

- put those IDs in logs/traces; keep metrics labels low-cardinality

## 4) Wrong: Dashboards With No Questions

Wrong pattern:

- 40 panels with no drill-down and no narrative

Right pattern:

- dashboards answer specific questions and support triage workflow

## 5) Wrong: Logs Contain Secrets

Wrong pattern:

- print tokens/credentials in logs during debugging

Right pattern:

- never print secrets; verify via behavior and scoped checks
