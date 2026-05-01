---
title: Cloud Extension Lab (Optional)
tags:
  - cloud
  - optional
  - terraform
module: "08"
---

# Cloud Extension Lab (Optional) — Terraform in Cloud

Core learning is local-first. This extension maps Terraform workflows to cloud.

## Goal

Practice:

- remote state backend with locking and encryption
- least-privilege credentials for Terraform
- plan in PR, apply with approvals
- drift detection and reconciliation policy

## Cost Control (Mandatory)

- set budgets and alerts before provisioning
- tag resources with owner/purpose/expires_on/module
- destroy resources immediately after verification

Read: [Lab Safety and Cost Control](../../00-HOW-TO-USE/03-lab-safety-cost-control.md)

## Steps (High Level)

1. Configure a remote backend (provider-specific) with:
   - locking
   - versioning/backups
2. Provision a small, low-cost resource (or use free-tier).
3. Introduce drift manually (small, reversible).
4. Run plan to detect drift and reconcile.
5. Destroy resources and verify deletion.

## Verification Signals

- lock prevents concurrent applies
- state is protected and recoverable
- drift is detected reliably

## Cleanup

- destroy resources
- delete state objects only after destroy and only if safe
