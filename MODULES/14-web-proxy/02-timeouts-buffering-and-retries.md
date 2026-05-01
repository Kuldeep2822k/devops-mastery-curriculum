---
title: Timeouts, Buffering, and Retries
tags:
  - web-proxy
  - timeouts
  - buffering
module: "14"
---

# Timeouts, Buffering, and Retries

## Timeouts (Most Common Root Cause)

Timeout categories:

- client → proxy (keepalive, read timeout)
- proxy → upstream connect timeout
- proxy → upstream read timeout

Symptoms:

- 504 Gateway Timeout
- intermittent 502/499 patterns (depending on proxy)

## Buffering

Buffering can:

- protect upstreams from slow clients
- increase latency for streaming responses
- increase memory usage on proxy

Operational guidance:

- decide per endpoint (streaming vs standard)

## Retries

Retries can help transient failures but can also amplify incidents.

Proxy retry risks:

- non-idempotent requests retried (duplicate side effects)
- retry storms under upstream degradation

Safe defaults:

- retry only safe/idempotent methods unless you explicitly support idempotency keys
- cap retry count and use backoff (or keep minimal proxy retries)

## Verification Signals

- compare upstream health vs proxy error rate
- check latency distributions before and after timeout changes
