---
title: Runbook Template (Module 14)
tags:
  - runbook
  - template
  - web-proxy
module: "14"
---

# Runbook Template — Module 14 Web Proxy

## Title

Proxy — <Symptom (502/503/504 / routing wrong / headers)>

## Impact

- user impact:
- scope:
- severity:

## Safety and Preconditions

- capture proxy logs and upstream logs before changes
- change one variable at a time

## Quick Triage

1) curl through proxy  
2) curl direct to upstream  
3) check listeners and routes  
4) check upstream health/endpoints  
5) check timeouts and header forwarding  

## Diagnosis

- 502: connect failures / wrong upstream
- 503: no healthy upstreams
- 504: upstream slow / timeouts

## Containment

- rollback proxy config
- route traffic away from failing upstream
- reduce retries/load

## Verification

- 200 responses through proxy
- error/timeout rates normal over a window
- headers correct (client IP, proto)

## Prevention

- timeout policy and runbook links
- config review and staged rollout
