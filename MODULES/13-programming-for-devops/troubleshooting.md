---
title: Troubleshooting Guide
tags:
  - troubleshooting
  - programming
module: "13"
---

# Troubleshooting — Module 13 Programming for DevOps

## Fast Diagnosis Checklist

- is the tool targeting the right environment/scope?
- is there a dry-run mode to confirm actions?
- are actions bounded (max items, time window)?
- are timeouts set for network calls?
- are retries bounded and only for transient errors?
- are logs sufficient to reconstruct actions?

## Common Failures

### Tool Hangs

Likely:

- missing timeouts

Fix:

- add timeouts for network and subprocess operations

### Tool Deletes Too Much

Likely:

- no bounds or unsafe defaults

Fix:

- require explicit `--apply`
- add `--max-*` limits and allowlists

### API Rate Limit Problems

Likely:

- high request volume or no Retry-After handling

Fix:

- respect 429 Retry-After
- cap concurrency and add backoff

### Duplicate Side Effects

Likely:

- non-idempotent writes

Fix:

- use idempotency keys or “upsert” logic
- store checkpoints
