---
title: 'Tool Alternatives Matrix'
tags:
  - appendix
  - tools
  - portability
---

# Tool Alternatives Matrix (Avoid Vendor Lock-In)

This guide is pattern-first. Use whichever tool fits your environment as long as you preserve the underlying operational behaviors (observability, rollback safety, provenance, least privilege).

## Examples

- Kubernetes local:
  - kind / minikube / k3d
- CI:
  - GitHub Actions / GitLab CI / Jenkins (patterns: stages, caching, artifacts, approvals)
- IaC:
  - Terraform / OpenTofu (patterns: state, drift, locking, modules)
- Config management:
  - Ansible / Salt / Puppet (patterns: idempotency, inventories, safe rollouts)
- Observability:
  - Prometheus+Grafana / OpenTelemetry stack / vendor platforms (patterns: SLIs/SLOs, alert hygiene)

