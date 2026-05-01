---
title: Cloud Extension Lab (Optional)
tags:
  - cloud
  - optional
  - web-proxy
module: "14"
---

# Cloud Extension Lab (Optional) — Managed Load Balancers

Core learning is local-first. This extension maps proxy behaviors to managed load balancers and ingress controllers.

## Goal

Practice:

- TLS termination and certificate rotation
- health checks and upstream pools
- timeouts and connection draining
- header policies and real client IP preservation

## Cost Control (Mandatory)

- set budgets and alerts before provisioning
- delete load balancers after verification

Read: [Lab Safety and Cost Control](../../00-HOW-TO-USE/03-lab-safety-cost-control.md)

## Verification Signals

- you can diagnose 502/503/504 with logs and health check evidence
- rollback of LB config is safe and fast
