---
title: Common Mistakes (Wrong vs Right)
tags:
  - mistakes
  - web-proxy
module: "14"
---

# Common Mistakes — Module 14 Web Proxy

## 1) Wrong: Random Timeout Tuning

Wrong pattern:

- increase timeouts without understanding upstream latency

Right pattern:

- measure upstream latency and set timeouts intentionally

## 2) Wrong: Retry Non-Idempotent Requests

Wrong pattern:

- proxy retries POSTs causing duplicates

Right pattern:

- retry only safe methods or require idempotency keys

## 3) Wrong: Trust X-Forwarded-For From Anyone

Wrong pattern:

- accept spoofed client IP

Right pattern:

- trust forwarded headers only from trusted proxy layers

## 4) Wrong: No Direct Upstream Test

Wrong pattern:

- only test via proxy

Right pattern:

- compare direct upstream vs proxy to isolate layer

## 5) Wrong: Change Proxy During Incident Without Evidence

Wrong pattern:

- guess changes

Right pattern:

- capture logs, errors, and timeouts; then change one variable at a time
