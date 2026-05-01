---
title: Cloud Extension Lab (Optional)
tags:
  - cloud
  - optional
  - foundations
module: "01"
---

# Cloud Extension Lab (Optional) — Foundations

Core learning is local-first. This extension maps the same operational behaviors to a minimal cloud deployment.

## Goal

Deploy the simple service to a cloud environment with:

- environment separation (dev vs prod-like)
- a basic health check endpoint exposed
- a rollback mechanism (previous artifact)
- strict cost controls

## Cost Control (Mandatory)

Before you create anything:

- set a budget with alerts
- tag every resource with:
  - owner
  - purpose
  - expires_on
  - module
- commit to deleting everything after verification

Do not run this lab until you have read: [Lab Safety and Cost Control](../../00-HOW-TO-USE/03-lab-safety-cost-control.md)

## Option A: AWS (High Level)

Suggested minimal mapping (choose one):

- ECS Fargate (container runtime with less cluster management)
- or EC2 + systemd (closer to “ops fundamentals”)

Operational tasks:

- deploy v1
- verify health endpoint
- deploy a regression (latency or failing health)
- rollback to v1

## Option B: Azure (High Level)

Suggested mapping:

- Azure Container Apps
- or VM + systemd

Same operational tasks as above.

## Option C: GCP (High Level)

Suggested mapping:

- Cloud Run
- or Compute Engine VM + systemd

Same operational tasks as above.

## Verification Signals (Cloud)

- health endpoint returns 200
- access logs show requests and status
- rollback returns metrics/logs to baseline state

## Cleanup

Delete everything created for this lab and verify deletion:

- service/app
- load balancer/ingress
- compute resources
- container images if they incur cost

Record cleanup verification in your evidence repo.
