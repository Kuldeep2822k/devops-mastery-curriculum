---
title: Cloud Extension Lab (Optional)
tags:
  - cloud
  - optional
  - delivery
module: "07"
---

# Cloud Extension Lab (Optional) — Delivery in Cloud Environments

Core learning is local-first. This extension maps delivery patterns to managed services.

## Goal

Practice a real promotion model in cloud:

- build artifact once (image digest)
- deploy to staging
- run smoke checks and observe telemetry
- promote the same digest to production
- execute rollback to previous digest

## Cost Control (Mandatory)

- set budgets and alerts before provisioning
- tag everything: owner, purpose, expires_on, module
- delete environments after verification

Read: [Lab Safety and Cost Control](../../00-HOW-TO-USE/03-lab-safety-cost-control.md)

## Option A: Kubernetes (Managed)

Use EKS/AKS/GKE and repeat Module 06/07 labs with:

- ingress controller and external routing
- real load balancer provisioning
- namespace separation for stage/prod

## Option B: Managed Container Runtime

Use ECS/Cloud Run/Container Apps and repeat:

- deploy v1 digest
- deploy v2 digest with controlled failure
- rollback by digest

## Verification Signals

- stage passes smoke checks and has stable error/latency signals
- prod deploy uses the same digest as stage
- rollback returns signals to baseline

## Cleanup

- delete compute/runtimes
- delete load balancers and public IPs
- delete images if they incur storage costs
