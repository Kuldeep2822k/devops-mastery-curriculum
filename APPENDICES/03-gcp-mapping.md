---
title: 'GCP Mapping'
tags:
  - appendix
  - gcp
  - cloud
---

# GCP Mapping (Local-First → GCP)

## Common Equivalents

- Containers:
  - Docker local → Artifact Registry (registry)
  - local compose → Cloud Run (optional) or GKE
- Kubernetes:
  - kind/minikube → GKE
- IaC:
  - Terraform local workflow → Terraform + Google provider
- Identity:
  - local tokens → Workload Identity (GKE) + IAM service accounts
- Observability:
  - local Prometheus/Grafana → Cloud Monitoring + Managed Service for Prometheus (optional)

## Cost Control Defaults

- Tag/label resources with owner + TTL.
- Prefer small node pools; avoid always-on NAT unless required.
- Verify cleanup: GKE cluster, load balancers, reserved IPs, persistent disks.

