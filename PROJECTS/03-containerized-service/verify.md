---
title: '03-containerized-service: Verify'
tags:
  - project
---

# 03-containerized-service — Verify

```bash
curl -fsS http://127.0.0.1:18080/healthz
curl -fsS http://127.0.0.1:18080/version
docker logs --tail 20 proj03
```

Expected:

- health returns ok
- version returns a value you set
