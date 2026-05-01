---
title: "Deep Dive 01: CI as an Attack Surface"
tags:
  - security
  - deep-dive
  - ci
module: "10"
---

# Deep Dive 01 — CI as an Attack Surface

## Why CI Is Dangerous

CI can:

- execute untrusted code (PRs)
- access secrets
- publish artifacts and deploy

This makes CI a prime target.

## Common Attacks

- PR exfiltrates secrets via logs/network calls
- dependency confusion and typosquatting
- cache poisoning
- malicious build scripts in dependencies

## Controls (Practical)

- no production secrets in PR jobs
- minimal permissions for CI tokens
- approvals for production deploy steps
- pin dependencies and base images
- store artifacts immutably and attach metadata

## Operator Habit

Treat CI failures and anomalies like production incidents:

- investigate
- contain
- rotate secrets if exposure suspected
