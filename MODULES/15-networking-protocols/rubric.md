---
title: Rubric (0–4)
tags:
  - rubric
  - networking
module: "15"
---

# Rubric — Module 15 Networking Protocols

Use global rubric meanings: [Evidence and Rubrics](../../00-HOW-TO-USE/04-evidence-rubrics.md)

## Skill 1: Failure Classification

- 0: Cannot distinguish failure modes.
- 1: Can run commands but misinterprets refused vs timeout vs TLS.
- 2: Correctly classifies failures and chooses next checks.
- 3: Builds fast decision trees and reduces MTTR.
- 4: Creates org-wide network debug playbooks and training.

## Skill 2: DNS Debugging

- 0: Blames “DNS” without evidence.
- 1: Can run dig/nslookup but struggles with caching/TTL.
- 2: Diagnoses resolution vs connectivity and split-horizon issues.
- 3: Designs DNS change practices and monitoring.
- 4: Prevents DNS-driven incidents via strong controls and runbooks.

## Skill 3: HTTP and TLS Debugging

- 0: Disables TLS verification as default.
- 1: Can curl but struggles with cert/SNI issues.
- 2: Uses openssl/curl to diagnose cert chain and handshake issues.
- 3: Connects TLS and proxy behaviors to user impact.
- 4: Designs secure-by-default network policies and incident response.

## Skill 4: Operational Writing

- 0: No runbooks.
- 1: Notes exist but not executable.
- 2: Runbook is executable with commands and expected signals.
- 3: Runbooks reduce MTTR and prevent recurrence.
- 4: Documentation becomes a platform asset across teams.
