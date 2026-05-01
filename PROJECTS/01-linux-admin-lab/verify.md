---
title: '01-linux-admin-lab: Verify'
tags:
  - project
---

# 01-linux-admin-lab — Verify

```bash
curl -fsS http://127.0.0.1:18070/healthz
systemctl --user status proj01.service --no-pager | head
journalctl --user -u proj01.service -n 20 --no-pager
```

Expected:

- health returns `ok`
- status shows Active: active
- logs show recent successful start
