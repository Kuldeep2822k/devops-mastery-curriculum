---
title: Troubleshooting Guide
tags:
  - troubleshooting
  - foundations
module: "01"
---

# Troubleshooting — Module 01 Foundations

Use the global framework: [Troubleshooting Framework](../../00-HOW-TO-USE/05-troubleshooting-framework.md)

## Fast Triage Checklist

- Confirm impact: what fails (health? latency? connection?)
- Confirm the service is running and listening
- Confirm configuration flags (env vars) are what you think
- Confirm logs show expected event lines
- Avoid restarts until evidence is captured

## Core Diagnosis Commands (Local)

### Process and Ports

```bash
ps aux | head
ss -tulpn
```

### Endpoint Checks

```bash
curl -i http://localhost:8080/healthz
curl -o /dev/null -s -w "%{http_code} %{time_total}\n" http://localhost:8080/healthz
```

### Environment Inspection

```bash
env | sort | head
env | grep -E '^(PORT|SERVICE_NAME|FAIL_HEALTH|LATENCY_MS)=' || true
```

## Common Failures

### Service Not Listening

Signals:

- curl connection refused
- no process bound to port

Likely causes:

- service not started
- crashed on startup
- bound to different port

Fix:

- start service and re-verify with `ss` and `curl`

### Health Failing (500)

Signals:

- curl shows 500
- logs show failing health events

Likely causes:

- intentional failure injection flag enabled
- real error in handler logic

Fix:

- disable fault flags and restart
- if persists, inspect app code and logs

### Latency Regression

Signals:

- HTTP 200 but time_total increases

Likely causes:

- intentional latency injection
- resource pressure on host

Fix:

- remove latency injection
- check host CPU/memory/disk pressure

## Escalation Guidance (Even When Solo)

If you are stuck:

- write down current hypothesis
- capture evidence (commands + outputs)
- revert to last known good state (rollback)
- reduce scope (test from host only, then from a local container later)
