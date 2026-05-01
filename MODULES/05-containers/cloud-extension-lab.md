---
title: Cloud Extension Lab (Optional)
tags:
  - cloud
  - optional
  - containers
module: "05"
---

# Cloud Extension Lab (Optional) — Containers in Cloud Runtimes

Core learning is local-first. This extension maps container operations to cloud runtimes.

## Goal

Deploy the same containerized service to a managed container runtime and practice:

- image publishing to a registry
- deploy by digest (immutable)
- rollback to a prior digest
- diagnosing image pull failures and auth issues

## Cost Control (Mandatory)

Before provisioning:

- set budgets and alerts
- tag resources with owner/purpose/expires_on/module
- delete everything after verification

Read: [Lab Safety and Cost Control](../../00-HOW-TO-USE/03-lab-safety-cost-control.md)

## Option A: AWS

Suggested mapping:

- ECR (registry)
- ECS Fargate (runtime)

Drills:

- deploy digest A, verify health
- deploy digest B with a breaking change, detect and rollback

## Option B: Azure

Suggested mapping:

- ACR (registry)
- Azure Container Apps

Same drills.

## Option C: GCP

Suggested mapping:

- Artifact Registry (registry)
- Cloud Run

Same drills.

## Verification Signals

- health endpoint returns 200
- deployed revision matches expected image digest
- rollback returns to known-good digest

## Cleanup

- delete service/runtime resources
- delete registry images if they incur costs
- verify no resources remain
