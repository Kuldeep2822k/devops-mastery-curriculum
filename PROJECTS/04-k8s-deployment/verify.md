---
title: '04-k8s-deployment: Verify'
tags:
  - project
---

# 04-k8s-deployment — Verify

```bash
kubectl -n proj04 rollout status deploy/web
kubectl -n proj04 get svc,ep
```

Expected:

- rollout completes
- service has endpoints
