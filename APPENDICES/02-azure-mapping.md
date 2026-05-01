---
title: 'Azure Mapping'
tags:
  - appendix
  - azure
  - cloud
---

# Azure Mapping (Local-First → Azure)

## Common Equivalents

- Containers:
  - Docker local → ACR (registry)
  - local compose → Container Apps (optional) or AKS
- Kubernetes:
  - kind/minikube → AKS
- IaC:
  - Terraform local workflow → Terraform + AzureRM provider
- Identity:
  - local tokens → Managed Identities + workload identity (AKS)
- Observability:
  - local Prometheus/Grafana → Azure Monitor / Managed Grafana (or self-host)

## Cost Control Defaults

- Tag everything with ownership + TTL.
- Prefer smallest AKS node pools and auto-shutdown where appropriate.
- Verify cleanup: AKS cluster, load balancers, public IPs, disks, and route tables.

