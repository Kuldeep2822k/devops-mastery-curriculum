---
title: "Lab 02: Alerts and Runbooks Drill (Design + Evidence)"
tags:
  - lab
  - observability
  - alerting
  - runbooks
module: "11"
---

# Lab 02 — Alerts and Runbooks Drill (Design + Evidence)

## Goal

Design:

- 2 alerts (one availability, one latency)
- 1 dashboard outline
- 1 runbook that answers “what do I do in the first 10 minutes?”

Then practice using your signals to debug a simulated incident.

## Prereqs

- Module 11 Lab 01 service (or any local service you can send requests to)

## Setup

Create an evidence folder for this drill:

```bash
mkdir -p ~/work/devops-labs/mod11-alerts
cd ~/work/devops-labs/mod11-alerts
```

Create a runbook draft you will fill in:

```bash
cat > runbook.md <<'EOF'
# Runbook

## Service Summary

## First 10 Minutes

## Expected Signals

## Containment / Rollback

## Evidence to Capture
EOF
```

## Steps

### 1) Define Two Alerts (Write Them Down)

Alert A (Availability):

- SLI: success rate
- Window: 5 minutes
- Trigger: error rate > 2% for 5 minutes
- Runbook link:
- Owner:

Alert B (Latency):

- SLI: p95 latency
- Window: 10 minutes
- Trigger: p95 > 500ms for 10 minutes
- Runbook link:
- Owner:

### 2) Define a Dashboard Outline

Panels:

- traffic
- errors (status class)
- latency (p50/p95/p99)
- saturation (CPU/mem/queue depth)
- deploy markers (version over time)

Drill-down links:

- from high error rate to logs
- from high latency to traces or slow request logs

### 3) Write a Runbook (Use Template)

Use: [runbook-template.md](runbook-template.md)

Minimum runbook sections:

- quick triage commands
- expected signals
- rollback trigger guidance
- evidence to capture before changes

### 4) Simulate Incident and Diagnose

If using Lab 01 service, generate:

- failures: call `/fail` repeatedly
- latency: call `/slow` repeatedly

Write down:

- what signals change first
- what hypothesis you form
- how you would contain (rollback, disable feature, reduce traffic)

## Verify

- Your alerts are actionable and point to a runbook.
- Your runbook contains:
  - first 10 minute steps
  - verification signals
  - containment and rollback guidance

## Cleanup

- save your alert and dashboard definitions in your notes

## Troubleshooting

### Symptom: alerts aren’t actionable

Fix:

- add a single clear “first action” per alert (containment / rollback / disable feature)
- ensure alerts link to a runbook section with commands and expected signals

### Symptom: dashboard doesn’t answer questions

Fix:

- write a question per panel (e.g., “is it traffic, errors, latency, or saturation?”)
- add drill-down from symptom → logs and/or slow request evidence

## Why This Matters in Production

- Alerts without runbooks create pager fatigue.
- Dashboards without questions become noise.

## Definition of Done

- You can explain why each alert exists, what it protects, and how you would respond.
