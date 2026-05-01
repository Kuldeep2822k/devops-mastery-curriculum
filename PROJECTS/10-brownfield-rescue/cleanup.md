---
title: '10-brownfield-rescue: Cleanup'
tags:
  - project
---

# 10-brownfield-rescue — Cleanup

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
