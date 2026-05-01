---
title: "Deep Dive 01: Timeouts, Retries, and the Network Failure Matrix"
tags:
  - networking
  - deep-dive
  - timeouts
module: "15"
---

# Deep Dive 01 — Timeouts, Retries, and the Network Failure Matrix

## The Failure Matrix

Most “network issues” are one of:

- DNS resolution failure
- connect timeout (routing/firewall)
- connection refused (no listener)
- TLS handshake failure
- read timeout (slow upstream)

Each failure should map to:

- a specific set of commands
- a specific containment plan

## Retry Risk

Retries without backoff:

- amplify load
- increase latency
- cause cascading failures

Safe strategy:

- bounded retries
- exponential backoff with jitter
- retry only idempotent operations unless idempotency keys used

## Staff-Level Guidance

Write a runbook that starts with classification:

- refused vs timeout vs TLS vs DNS

This reduces MTTR dramatically.
