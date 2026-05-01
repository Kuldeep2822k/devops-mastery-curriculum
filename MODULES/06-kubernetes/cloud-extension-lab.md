---
title: Cloud Extension Lab (Optional)
tags:
  - cloud
  - optional
  - kubernetes
module: "06"
---

# Cloud Extension Lab (Optional) — Managed Kubernetes

Core learning is local-first. This extension maps the same debugging and operational behaviors to managed Kubernetes.

## Goal

On a managed cluster, practice:

- node and pod visibility
- image pull credentials and registry auth
- Service/Ingress routing
- RBAC troubleshooting
- resource requests/limits and Pending pods

## Cost Control (Mandatory)

- create a dedicated learning account/subscription/project
- set budgets and alerts before provisioning
- tag resources with owner/purpose/expires_on/module
- delete cluster immediately after verification

Read: [Lab Safety and Cost Control](../../00-HOW-TO-USE/03-lab-safety-cost-control.md)

## Option A: AWS (EKS)

High-level mapping:

- EKS cluster + managed node group
- ingress controller (as required)

Drills:

- deploy an app and intentionally break labels/probes
- simulate ImagePullBackOff via wrong image tag
- validate RBAC denies using a restricted role

## Option B: Azure (AKS)

Same drills:

- deploy/break/fix
- DNS and service endpoints
- RBAC troubleshooting

## Option C: GCP (GKE)

Same drills:

- deploy/break/fix
- ingress route issues
- resource/scheduling diagnosis

## Verification Signals

- events and describe output provide clear root cause
- rollback restores service quickly
- RBAC deny is explained and resolved via least privilege

## Cleanup

- delete cluster and node pools
- delete load balancers and public IPs created by ingress
- verify no persistent volumes and disks remain
