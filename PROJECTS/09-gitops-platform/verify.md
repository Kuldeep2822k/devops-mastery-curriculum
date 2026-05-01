---
title: '09-gitops-platform: Verify'
tags:
  - project
---

# 09-gitops-platform — Verify

```bash
./gate.sh
sha256sum env/dev/app.txt env/prod/app.txt
```

Expected:

- gate passes and artifacts match
