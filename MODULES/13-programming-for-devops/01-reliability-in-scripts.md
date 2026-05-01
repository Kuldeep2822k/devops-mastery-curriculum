---
title: Reliability in Scripts (Idempotency, Safety, Observability)
tags:
  - programming
  - automation
  - reliability
module: "13"
---

# Reliability in Scripts (Idempotency, Safety, Observability)

## Safety Properties for Ops Scripts

- idempotent: safe to rerun
- bounded: limits scope and damage
- observable: logs actions and results (without secrets)
- reversible: rollback path exists

## CLI UX Matters

Good CLI defaults:

- dry-run mode
- explicit target selection
- clear exit codes
- structured output option (JSON)

## Error Handling and Timeouts

Operational scripts must:

- set timeouts (network and subprocess)
- handle partial failures
- avoid infinite retries

## Logging Without Leaking

- log what you did (resource IDs, counts)
- do not log secrets (tokens, full payloads with credentials)

## Anti-Patterns

- “works on my machine” scripts with hidden dependencies
- `rm -rf` style destructive operations without guards
- scripts that silently ignore errors
