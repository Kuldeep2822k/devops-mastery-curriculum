---
title: '04-k8s-deployment: Cleanup'
tags:
  - project
---

# 04-k8s-deployment — Cleanup

## Steps

- Stop any services/containers/clusters created by this project
- Remove the project workspace

## Verify Cleanup

```bash
ps aux | head
docker ps 2>/dev/null || true
kubectl get ns 2>/dev/null || true
```

Expected:

- no project resources remain
