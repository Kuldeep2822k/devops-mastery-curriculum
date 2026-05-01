---
title: Operational Safety (Blast Radius, Privilege, Rollback)
tags:
  - ansible
  - safety
module: "09"
---

# Operational Safety (Blast Radius, Privilege, Rollback)

## Blast Radius Control

Techniques:

- target small host subsets first (`--limit`)
- serial execution for risky changes (`serial: 1`)
- canary host group

## Privilege and Become

Use privilege escalation (`become`) only where required.

Risk:

- running everything as root increases blast radius and audit risk

## Rollback Strategy

Rollback for config management means:

- versioned config templates
- ability to redeploy previous template quickly
- service reload/restart procedure

## Verification and Post-Change Monitoring

Automation success is not service success.

After changes, verify:

- config validity
- service health endpoints
- logs show normal behavior

## Anti-Patterns

- running playbooks against “all” without limits
- editing production manually and letting playbooks “fix it later”
- no rollback plan for config changes
