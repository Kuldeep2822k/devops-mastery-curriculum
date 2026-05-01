---
title: Secrets Hygiene (Operators)
tags:
  - security
  - secrets
module: "10"
---

# Secrets Hygiene (Operators)

## Rules (Non-Negotiable)

- Never commit secrets.
- Never print secrets in logs.
- Treat “temporary tokens” as secrets too.
- Rotate secrets immediately if leaked.

## Where Secrets Leak

- shell history
- CI logs (echo/env dumps)
- config files committed to git
- Terraform state files
- screenshots and incident chat logs

## Safer Patterns

- runtime injection (secret manager, CI secret store)
- least privilege tokens scoped to a single purpose
- short-lived credentials and revocation
- separate secrets per environment (dev/stage/prod)

## Detection Signals

- secret scanning on commits/PRs
- alerting on suspicious access patterns
- audit logs for credential use

## Incident Playbook (If a Secret Leaks)

1) assume compromise  
2) rotate/revoke the secret  
3) remove the secret from code and configs  
4) identify where it leaked (logs, forks, artifacts)  
5) add guardrails to prevent recurrence  
