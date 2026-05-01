---
title: "Why Python Tools"
tags:
  - tooling
  - python
---

# Why Python Tools

This guide uses small Python scripts in labs because they are:

- portable across environments
- easy to inspect and modify
- suitable for safe automation patterns (timeouts, retries, idempotency, dry-run)

## Rules

- Scripts must be safe by default (no destructive behavior without explicit flags).
- Verification must be command-based with expected signals.
- Never print or hardcode secrets.

