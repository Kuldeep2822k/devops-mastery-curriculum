---
title: '05-terraform-platform: Verify'
tags:
  - project
---

# 05-terraform-platform — Verify

```bash
terraform state list
terraform plan
```

Expected:

- state contains expected resources
- plan is empty or non-destructive after refactor
