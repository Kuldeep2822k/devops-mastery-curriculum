---
title: HTTP APIs and Resilience (Timeouts, Retries, Rate Limits)
tags:
  - programming
  - http
  - apis
module: "13"
---

# HTTP APIs and Resilience (Timeouts, Retries, Rate Limits)

## Timeouts Are Mandatory

Default HTTP timeouts are often too long or absent.

Always set:

- connect timeout
- read timeout

## Retries With Backoff

Retry only on transient failures:

- timeouts
- 429 rate limits
- 5xx from upstream

Avoid retrying:

- 4xx validation errors (usually permanent)

Backoff:

- exponential backoff with jitter

## Pagination and Limits

APIs often require:

- pagination
- server-side limits

Operator habit:

- bound total items processed
- store checkpoints for resumability

## Idempotency Keys

For write APIs:

- use idempotency keys if available
- design your script so repeated runs don’t duplicate side effects

## Anti-Patterns

- no timeouts
- unbounded retries
- “fetch everything” without limits
