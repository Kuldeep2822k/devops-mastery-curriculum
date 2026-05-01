---
title: Troubleshooting Framework
tags:
  - troubleshooting
  - incident-response
---

# Troubleshooting Framework

This framework is designed for on-call reality: ambiguous symptoms, time pressure, incomplete telemetry, and non-obvious dependencies.

## Golden Rules

- Stop guessing. Observe signals first.
- Change one thing at a time (unless containment is needed).
- Prefer reversible actions.
- Log every action with a reason (you will forget under pressure).
- If you cannot explain a hypothesis, you are not diagnosing.

## The Triage Ladder (Fast → Deep)

### 1) Define the Impact (2–5 minutes)

Capture:

- what is broken (user-visible symptom)
- which scope (single user, region, all traffic)
- when it started (deploy? config change? dependency event?)
- current severity (SEV and business impact)

### 2) Establish Ground Truth (5 minutes)

Prefer these signals:

- error rate, latency, saturation (RED)
- resource constraints: CPU/mem/disk/network
- recent deploy/config changes
- dependency health (DNS/TLS/upstream)

### 3) Contain Blast Radius (If Needed)

Containment options (choose the least risky):

- rollback to last known good artifact (preferred)
- reduce traffic (rate limiting, feature flag off)
- scale out to reduce saturation (only if safe)
- isolate a bad node/instance/pod

Containment is not a fix; it buys time.

### 4) Diagnose with Hypotheses

Write 2–3 hypotheses based on the strongest signals.

Example pattern:

- Symptom: 503 at ingress
- Signals: upstream connect timeouts; pods Ready; node CPU high
- Hypotheses: (a) app hung, (b) node pressure causing drops, (c) DNS issues to upstream

### 5) Execute the Minimal Fix

Prefer:

- rollback over forward-fix during high severity
- config change over code change (if safe and reversible)
- reducing load over scaling complexity

### 6) Verify and Monitor

Verification is not “it seems fine”:

- error rate drops and stays down
- latency improves
- saturation reduces
- synthetic check passes
- logs show expected successful requests

### 7) Prevent Recurrence

Preventive work is part of “done”:

- add a diagnostic runbook
- add missing alerts or remove noisy ones
- improve dashboards
- add guardrails (policy checks, safe deploy gates)
- add load tests or chaos tests where appropriate

## Default Command Sets (By Layer)

Use these as starting points; modules provide deeper, specific commands.

### Linux/Host

- `uptime`
- `free -m`
- `df -h`
- `ss -tulpn`
- `journalctl -u <service> --since "15 min ago"`
- `top` or `htop`

### Containers

- `docker ps`
- `docker logs <container>`
- `docker inspect <container>`
- `docker stats`

### Kubernetes

- `kubectl get pods -A`
- `kubectl describe pod <pod>`
- `kubectl logs <pod> --previous`
- `kubectl get events --sort-by=.lastTimestamp`
- `kubectl exec -it <pod> -- sh`
- `kubectl get endpoints,svc,ingress -A`

### CI/CD

- read pipeline logs for the first failure, not the last
- identify which step produced/consumed which artifact
- check cache keys and environment-specific secrets

### Networking/TLS

- `dig <name>`
- `curl -v https://...`
- `openssl s_client -connect host:443 -servername host`
- trace route and connectivity tests where allowed

## Time-Boxing Guidance

- 10 minutes: containment decision (rollback/traffic reduction) if severe
- 15 minutes: if no root cause yet, expand hypotheses and check dependencies
- 30 minutes: escalate/ask for help; update incident comms; avoid thrash

## What to Write in an Incident Log

- timestamps with actions
- hypothesis at the time you took each action
- commands run and outputs (redact secrets)
- what changed and why
- verification signals after changes
