---
title: "Lab 02: Change and Rollback Drill (Local)"
tags:
  - lab
  - foundations
  - incident
module: "01"
---

# Lab 02 — Change and Rollback Drill (Local)

## Goal

Practice safe change with rollback using a service you control:

- introduce a breaking change (latency or failed health)
- detect impact using signals
- perform containment and rollback
- write an incident timeline and follow-ups

## Prereqs

- Lab 01 completed (service exists)
- `curl` available
- basic familiarity with the troubleshooting framework: [framework](../../00-HOW-TO-USE/05-troubleshooting-framework.md)

## Setup

Go to your lab directory:

```bash
cd ~/work/devops-labs/mod01-service
```

Start the service (terminal A):

```bash
./run.sh
```

Create a simple “change log” file:

```bash
printf "ts,change,reason\n" > change-log.csv
```

## Steps

### 1) Establish Baseline (Ground Truth)

Run 20 health checks (terminal B):

```bash
for i in $(seq 1 20); do curl -fsS http://localhost:8080/healthz >/dev/null; done
echo ok
```

Capture a baseline latency sample:

```bash
for i in $(seq 1 5); do curl -o /dev/null -s -w "%{time_total}\n" http://localhost:8080/healthz; done
```

Expected signals:

- all requests succeed
- latencies are low and consistent

Write a change log entry:

```bash
date +%s | awk '{print $1 ",baseline,baseline established"}' >> change-log.csv
```

### 2) Introduce a Risky Change (Latency Injection)

Restart the service with added latency (terminal A):

- stop with Ctrl+C
- start with environment variable:

```bash
export LATENCY_MS=800
./run.sh
```

Log the change:

```bash
date +%s | awk '{print $1 ",latency_inject,simulate regression"}' >> change-log.csv
```

### 3) Detect Impact Using Signals

Run latency probes:

```bash
for i in $(seq 1 5); do curl -o /dev/null -s -w "%{http_code} %{time_total}\n" http://localhost:8080/healthz; done
```

Expected signals:

- HTTP 200 continues
- latency increases to roughly 0.8s

### 4) Containment Decision (Time-Boxed)

If this were production with an SLO like “p95 < 300ms”, you would treat this as severe.

Containment options locally:

- rollback the change (preferred)
- reduce load (not meaningful locally)

Choose rollback.

### 5) Rollback

Stop the service and unset the latency:

```bash
unset LATENCY_MS
./run.sh
```

Log:

```bash
date +%s | awk '{print $1 ",rollback,restore baseline latency"}' >> change-log.csv
```

### 6) Verify Recovery

Run the same latency probes:

```bash
for i in $(seq 1 5); do curl -o /dev/null -s -w "%{http_code} %{time_total}\n" http://localhost:8080/healthz; done
```

Expected signals:

- latency returns to baseline range

## Verify

### Verify Signal-Based Decision Making

Open `change-log.csv` and confirm you captured:

- baseline timestamp
- change injection timestamp
- rollback timestamp

```bash
cat change-log.csv
```

Expected signals:

- three rows present (baseline, inject, rollback)

### Verify Logs Reflect Events

Observe logs in terminal A:

- start event after rollback
- health events show status ok

## Cleanup

Stop the service (Ctrl+C) and remove injected env vars:

```bash
unset LATENCY_MS
unset FAIL_HEALTH
```

Verify no listener:

```bash
ss -tulpn | grep -E ':(8080)\b' || true
```

## Troubleshooting

### Symptom: latency remains high after rollback

Diagnosis:

- confirm env var is actually unset:

```bash
env | grep -E '^LATENCY_MS=' || true
```

Fix:

- fully stop the process and start fresh with no env var

### Symptom: curl shows 500 unexpectedly

Diagnosis:

- check FAIL_HEALTH:

```bash
env | grep -E '^FAIL_HEALTH=' || true
```

Fix:

- unset and restart

## Why This Matters in Production

- Many incidents are regressions: latency, error rate, timeouts.
- Rollback is the safest containment when the blast radius is large.
- A change log and timeline are critical for coordination and for later root cause analysis.

## What to Write in a Runbook

- How to measure baseline latency (commands)
- What threshold triggers containment/rollback
- The rollback action and verification signal

## Definition of Done

- You can detect latency regression via repeated probes.
- You can rollback and verify recovery via the same probes.
- You can produce a minimal change log with timestamps and reasons.
