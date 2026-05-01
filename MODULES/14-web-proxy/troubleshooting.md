---
title: Troubleshooting Guide
tags:
  - troubleshooting
  - web-proxy
module: "14"
---

# Troubleshooting — Module 14 Web Proxy

## Fast Triage

1) reproduce through proxy and direct to upstream  
2) classify status (502/503/504)  
3) check proxy logs and upstream logs  
4) verify routing and upstream pool health  
5) verify timeouts and header policies  

## Useful Commands (Local)

- direct upstream:
  - `curl -v http://127.0.0.1:<upstream_port>/`
- through proxy:
  - `curl -v http://127.0.0.1:<proxy_port>/`
- listener check:
  - `ss -tulpn | grep -E ':(<port>)\\b' || true`

## Symptom Patterns

### 502

- upstream not reachable
- wrong port
- DNS failure

### 503

- no healthy upstreams
- empty endpoints

### 504

- upstream slow
- timeout too low

## Containment

- rollback proxy config
- route traffic away from failing upstream pool
- reduce load and retries if storm
