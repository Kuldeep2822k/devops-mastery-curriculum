---
title: Runbook Template (Module 13)
tags:
  - runbook
  - template
  - programming
module: "13"
---

# Runbook Template — Module 13 Programming for DevOps

## Title

Automation Tool — <Incident (runaway deletes / API rate limiting / bad target)>

## Impact

- what broke:
- scope:
- severity:

## Safety and Preconditions

- stop the tool if it is causing harm
- do not print secrets
- preserve evidence (targets processed, counts, errors)

## Quick Triage

- confirm tool version and command invoked
- confirm environment and target scope
- identify whether tool is bounded (max limits)
- identify timeouts and retry behavior

## Containment

- stop execution
- revoke/rotate tokens if leaked
- reduce traffic to upstream API if needed

## Fix

- add dry-run default and explicit apply
- add allowlists and max limits
- add timeouts and safe retries

## Verification

- rerun in dry-run shows correct targets
- apply run affects only intended scope
- upstream errors and rate limits stabilize

## Prevention

- policy: no production automation without dry-run and bounds
- tests for critical paths
