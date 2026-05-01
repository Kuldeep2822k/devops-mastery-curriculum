---
title: '02-git-ci-demo: Verify'
tags:
  - project
---

# 02-git-ci-demo — Verify

```bash
make ci
test -s dist/app.txt
test -s dist/manifest.txt
```

Expected:

- artifacts exist under dist/
- pipeline exits 0
