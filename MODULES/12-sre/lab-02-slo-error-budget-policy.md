---
title: "Lab 02: SLO + Error Budget Policy"
tags:
  - lab
  - sre
  - slo
module: "12"
---

# Lab 02 — SLO + Error Budget Policy

## Goal

Define:

- one SLO for a service
- an error budget policy that changes behavior when budget is burning/exhausted
- a release gating rule that uses the policy

## Prereqs

- pick a service (real or demo)

## Setup

Create a workspace for the SLO artifacts:

```bash
mkdir -p ~/work/devops-labs/mod12-slo
cd ~/work/devops-labs/mod12-slo
```

Create a policy doc you will fill in:

```bash
cat > SLO_POLICY.md <<'EOF'
# SLO + Error Budget Policy

## Service

## User Journey

## SLI

## SLO

## Error Budget Policy

## Release Gating Rules

## Alerts + Runbook Links
EOF
```

## Steps

### 1) Choose a User Journey

Examples:

- “GET /checkout succeeds”
- “job X completes within 10 minutes”

### 2) Define SLI and SLO

Write:

- SLI definition
- measurement method (logs/metrics)
- window (e.g., 30 days)
- target (e.g., 99.9%)

### 3) Define Error Budget Policy

Policy must state:

- what happens when burn rate is high
- who can override and how (break-glass)
- what must be verified after override

Example policy:

- budget healthy: normal deploys
- budget burning: require canary + extra approvals
- budget exhausted: freeze risky deploys; focus on reliability work

### 4) Define Alerts Tied to Budget

Define:

- fast-burn alert
- slow-burn alert

### 5) Write the ADR

Use template: [decision-record-template.md](decision-record-template.md)

## Verify

- SLO is measurable and tied to user value
- policy is explicit and actionable
- alert definitions have runbook links

## Cleanup

Save your artifacts:

```bash
ls -la
```

Expected:

- `SLO_POLICY.md` exists (and any ADR you created)

## Troubleshooting

### Symptom: SLO isn’t measurable

Fix:

- pick a signal source (logs vs metrics) and define the exact query or log filter

### Symptom: policy is vague (“be careful”)

Fix:

- write explicit gates (canary required, approvals required, freeze conditions)
- define a verification window and rollback trigger

## Why This Matters in Production

- SLOs without policy are posters.
- policy turns measurement into decision-making.

## Definition of Done

- you have an SLO and a policy that changes release behavior.
