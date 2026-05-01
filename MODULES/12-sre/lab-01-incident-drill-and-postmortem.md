---
title: "Lab 01: Incident Drill + Postmortem"
tags:
  - lab
  - sre
  - incident
module: "12"
---

# Lab 01 — Incident Drill + Postmortem

## Goal

Run a time-boxed incident drill and produce:

- timeline with evidence and decisions
- containment action
- verification window
- postmortem with prevention tasks

## Prereqs

- any service you can break safely (local HTTP service, or a Kubernetes demo service)

## Scenario (Example)

- a deploy introduced latency and intermittent 500s
- users report slowness and failures

## Setup

Create a drill workspace and evidence folder:

```bash
mkdir -p ~/work/devops-labs/mod12-incident
cd ~/work/devops-labs/mod12-incident
mkdir -p evidence
```

Create a timeline template:

```bash
cat > evidence/timeline.md <<'EOF'
# Incident Timeline

- T+00:00 page received (symptom/impact)
- T+00:05 initial hypotheses
- T+00:10 containment decision
- T+00:20 verification window begins
- T+00:40 incident resolved
EOF
```

## Steps

### 1) Declare Incident

Write:

- impact statement
- initial severity
- roles (IC, responder, comms)

### 2) Time-Boxed Triage (10 minutes)

Capture evidence:

- error rate and latency window
- recent deploy/version changes
- logs for failing requests
- saturation signals

Write your hypothesis and the evidence for/against it.

### 3) Choose Containment

Pick one:

- rollback to known good
- disable feature flag
- reduce traffic/load

Execute containment and record the exact action.

### 4) Verify Recovery (10–20 minutes)

Record:

- which signals improved
- when they improved
- any remaining degradations

### 5) Write Postmortem

Minimum sections:

- timeline (facts)
- root cause and contributing factors
- what went well / what didn’t
- action items with owners and deadlines

## Verify

- timeline exists with timestamps
- containment decision justified by evidence
- postmortem contains at least 5 concrete prevention actions

## Cleanup

- revert any lab changes
- close incident with final status update

## Troubleshooting

### Symptom: you can’t reproduce or measure the incident

Fix:

- switch to a smaller controlled blast radius (local service with `/fail` and `/slow`)
- write down what evidence you lack and what instrumentation you would add

### Symptom: you can’t decide rollback vs fix forward

Fix:

- define a decision rule up front (error budget burn, error rate threshold, verification window)

## Why This Matters in Production

- incident response is a skill; drills build calm behavior under pressure.

## Definition of Done

- you can run the drill end-to-end and produce prevention work, not just a narrative.
