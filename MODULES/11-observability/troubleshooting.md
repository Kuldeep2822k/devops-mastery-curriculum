---
title: Troubleshooting Guide
tags:
  - troubleshooting
  - observability
module: "11"
---

# Troubleshooting — Module 11 Observability

## Fast Triage Checklist

- confirm impact (availability/latency/correctness)
- identify what changed (deploy version/config)
- check golden signals (traffic/errors/latency/saturation)
- identify dependency failures and retry storms
- capture evidence before rollback/restart

## Evidence Sources

- metrics: error rate, latency, saturation, queue depth
- logs: structured events with request IDs
- traces: slow spans and dependency calls
- deploy markers: version changes over time

## Common Patterns

### High Error Rate

Likely:

- bad deploy/config
- dependency down
- permission/auth failures

Containment:

- rollback or disable feature flag

### High Latency

Likely:

- dependency slowness
- CPU throttling or saturation
- retry storm

Containment:

- reduce load, disable expensive feature, scale cautiously

### Alert Storm

Likely:

- one root cause triggers many symptom alerts

Fix:

- identify top-level symptom and suppress secondary alerts
