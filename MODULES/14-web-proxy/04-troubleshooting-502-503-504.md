---
title: Troubleshooting 502/503/504
tags:
  - web-proxy
  - troubleshooting
module: "14"
---

# Troubleshooting 502/503/504

## 502 Bad Gateway

Usually means:

- upstream connection failed
- upstream closed connection unexpectedly
- proxy misconfigured upstream address/port

First checks:

- can proxy resolve upstream DNS?
- can proxy connect to upstream port?
- does upstream listen and respond directly?

## 503 Service Unavailable

Usually means:

- no healthy upstreams
- upstream pool empty
- health check failing

First checks:

- upstream health status and endpoints
- label/selector issues (if proxy fronts Kubernetes ingress)

## 504 Gateway Timeout

Usually means:

- upstream too slow or hung
- timeout too aggressive

First checks:

- upstream response time directly
- proxy timeouts vs upstream latency
- dependency slowness causing upstream latency

## Operator Debug Playbook

1) reproduce with curl (through proxy and direct)  
2) check proxy logs for upstream errors and timings  
3) check upstream logs and health  
4) inspect timeouts and buffering config  
5) contain (rollback config or route traffic away)  
