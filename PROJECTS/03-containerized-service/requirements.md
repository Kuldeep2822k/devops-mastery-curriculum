---
title: 'Requirements'
tags:
  - project
---

# Requirements

## Deliverables

- Evidence folder with commands run + outputs (logs, screenshots optional)
- One runbook (runbook.md)
- One ADR section in runbook.md (decision + tradeoffs)
- One incident timeline + postmortem (postmortem-template.md)

## Constraints

- Local-first by default
- No plaintext secrets
- Every destructive action must have a cleanup verification step

## Acceptance Criteria

- A reviewer can follow steps.md end-to-end
- verify.md lists commands and expected signals (not just “works”)
- cleanup.md lists proof commands that nothing is left running
