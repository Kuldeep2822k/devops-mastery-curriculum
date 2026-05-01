---
title: 'Cloud Provider Mappings'
tags:
  - appendix
  - cloud
---

# Cloud Provider Mappings

Map local-first labs to one provider (AWS/GCP/Azure) using equivalent services. Keep costs controlled and verify cleanup.

## Example Mapping Skeleton

- compute: container → managed container service
- orchestration: kind → managed Kubernetes
- identity: local tokens → workload identity/IAM roles
- artifacts: local digest → registry + signing + SBOM
