---
title: "Deep Dive 02: Observability During Incidents"
tags:
  - observability
  - deep-dive
  - incident
module: "11"
---

# Deep Dive 02 — Observability During Incidents

## Preserve Evidence Before You Change Things

Before restart/rollback:

- capture current error rate and latency window
- capture last 15–30 minutes of logs for the service
- capture top events in the environment (deploy markers)
- capture trace examples if available

This enables:

- root cause analysis later
- prevention work after recovery

## Common Incident Patterns

- silent failures: logs missing due to misconfigured logging
- alert storms: one root cause triggers many pages
- “green dashboards” but broken: wrong SLI definition

## Staff-Level Behavior

- state hypotheses and update with evidence
- contain when needed (rollback, reduce traffic)
- write down the exact signals that triggered decisions

## Post-Incident Work

- prune noisy alerts
- improve runbooks with proven first commands
- add missing signals (version markers, dependency health)
