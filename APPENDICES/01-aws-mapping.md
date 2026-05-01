---
title: 'AWS Mapping'
tags:
  - appendix
  - aws
  - cloud
---

# AWS Mapping (Local-First → AWS)

Use this to translate local-first labs into AWS equivalents. Keep cloud labs optional and cost-controlled.

## Common Equivalents

- Containers:
  - Docker local → ECR (registry)
  - local compose → ECS Fargate (optional) or EKS
- Kubernetes:
  - kind/minikube → EKS
- IaC:
  - Terraform local workflow → Terraform + AWS provider
- Identity:
  - local service accounts → IAM roles + IRSA (EKS)
- Observability:
  - local Prometheus/Grafana → Managed Prometheus / Managed Grafana (or self-host on EKS)

## Cost Control Defaults

- Always tag everything with:
  - `owner`, `purpose`, `ttl`, `cost-center`
- Prefer free-tier sized instances and short TTLs.
- Verify cleanup:
  - EKS clusters, node groups, load balancers, NAT gateways, and EBS volumes are deleted.

