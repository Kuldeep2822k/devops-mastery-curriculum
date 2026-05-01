---
title: "Deep Dive 02: Rate Limiting, Backpressure, and Retry Storms"
tags:
  - programming
  - deep-dive
  - resilience
module: "13"
---

# Deep Dive 02 — Rate Limiting, Backpressure, and Retry Storms

## Retry Storm Pattern

When an upstream slows down:

- clients retry aggressively
- load increases
- upstream slows further

Your script can accidentally become a DoS tool.

## Backpressure Strategies

- respect 429 and Retry-After
- cap concurrency
- exponential backoff with jitter
- fail fast when budget exceeded

## Batch Processing

For large jobs:

- checkpoint progress
- process in chunks
- resume safely after failure

## Staff-Level Guidance

If a tool touches production APIs:

- treat it like a service
- define budgets (requests/minute, timeouts, retries)
