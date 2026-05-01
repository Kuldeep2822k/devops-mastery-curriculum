---
title: 'Troubleshooting Guide'
tags:
  - troubleshooting
  - artifacts
  - sbom
  - provenance
module: "17"
---

# Troubleshooting — Module 17 Artifacts (SBOM/Provenance/Signing)

## Triage Flow

- confirm scope (single user vs region vs global)
- check recent changes (deploys, config, credentials, dependencies)
- gather one fast signal (health endpoint, logs, metrics)
- isolate layers (client → edge → service → dependency → data)
- mitigate safely (reduce scope, revert, add capacity) then fix

## What to Record

- hypothesis and next check before each command
- the command and the observed result
- rollback point and what would trigger it
