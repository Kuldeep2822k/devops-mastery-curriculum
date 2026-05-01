---
title: Runbook Template (Module 02)
tags:
  - runbook
  - template
  - linux
module: "02"
---

# Runbook Template — Module 02 Linux

## Title

<Service> — <Symptom (e.g., not listening / restart loop / high latency)>

## Impact

- User impact:
- Scope:
- Severity:

## Safety and Preconditions

- Confirm environment/host:
- Evidence to capture before changes:
- Restart budget:

## Quick Triage (5 minutes)

- Process:
  - Command: `ps -eo pid,cmd,%cpu,%mem --sort=-%cpu | head -n 15`
  - Expected:
- Listener:
  - Command: `ss -tulpn`
  - Expected:
- Logs:
  - Command: `journalctl -u <service> --since "15 min ago" --no-pager | tail -n 200`
  - Expected:
- Resources:
  - Command: `uptime; df -h | head -n 20; free -m 2>/dev/null || true`
  - Expected:

## Diagnosis

### Hypotheses

- H1:
- H2:
- H3:

### Evidence and Interpretation

- Command:
  - Output signal:
  - Interpretation:

## Containment

Choose one:

- rollback deploy/unit config
- stop runaway process
- reduce load / disable feature

Steps:

1.

Verification:

- Command:
- Expected:

## Fix

Steps:

1.

Verification:

- Command:
- Expected:

## Post-Fix Monitoring

- watch for:
- duration:
- rollback trigger:

## Prevention / Follow-Ups

- guardrail:
- alert/dashboard:
- runbook improvement:
