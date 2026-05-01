---
title: Troubleshooting Guide
tags:
  - troubleshooting
  - sre
module: "12"
---

# Troubleshooting — Module 12 SRE

## Incident Triage Defaults

- confirm impact and severity
- assign roles and start timeline
- identify recent changes
- use observability signals to choose containment

## Containment Options

- rollback to known good artifact
- disable feature flag
- reduce traffic/load
- isolate failing dependency

## Verification Window

Do not declare recovery instantly:

- watch key signals for 10–30 minutes
- ensure alerts stop and user checks pass

## Post-Incident Checklist

- write postmortem within 24–72 hours
- create prevention tasks
- prune noisy alerts
- improve runbooks with proven first steps

## Common Failure Modes

- “we don’t know what version is deployed”
- “rollback didn’t help” due to schema/config incompatibility
- retry storms amplify outages
