---
title: 'Troubleshooting Guide'
tags:
  - troubleshooting
  - service-mesh
  - mtls
  - traffic
module: "18"
---

# Troubleshooting — Module 18 Service Mesh

## Triage Flow

- confirm scope (single user vs region vs global)
- check recent changes (deploys, config, credentials, dependencies)
- gather one fast signal (health endpoint, logs, metrics)
- isolate layers (client → edge → service → dependency → data)
- mitigate safely (reduce scope, revert, add capacity) then fix

## What to Record

- hypothesis and next check before each command
- the command and the observed result
- rollback point and what would trigger it
