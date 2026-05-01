---
title: Cloud Extension Lab (Optional)
tags:
  - cloud
  - optional
  - security
module: "10"
---

# Cloud Extension Lab (Optional) — Cloud Security Controls

Core learning is local-first. This extension maps security controls to cloud services.

## Goal

Practice:

- workload identity / least privilege for CI and workloads
- secret manager usage (no plaintext secrets)
- audit logs and detection signals
- policy-as-code gates for IaC and deployments

## Cost Control (Mandatory)

- set budgets and alerts before provisioning
- tag resources with owner/purpose/expires_on/module
- delete resources after verification

Read: [Lab Safety and Cost Control](../../00-HOW-TO-USE/03-lab-safety-cost-control.md)

## Steps (High Level)

1. Store a secret in a managed secret store.
2. Configure a workload identity to read it (least privilege).
3. Confirm secret is not present in CI logs or repo.
4. Enable audit logs and verify access is logged.
5. Implement one policy gate for deploys (approval + restricted branch).

## Verification Signals

- secret access requires correct identity and is audited
- CI and runtime do not log secrets
- deploys blocked when policy fails

## Cleanup

- delete secret, identities, policies, and resources
