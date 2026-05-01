---
title: HTTP Behavior and Debugging
tags:
  - networking
  - http
module: "15"
---

# HTTP Behavior and Debugging

## Status Codes (Operator View)

- 2xx: success
- 3xx: redirects (can cause loops)
- 4xx: client-side errors (auth, validation)
- 5xx: server-side errors (upstream failures, timeouts)

## Keepalive and Connection Reuse

Keepalive improves performance but can expose:

- connection pooling issues
- upstream resets
- load balancer idle timeout mismatches

## Debug Tools

- `curl -v` for headers, redirects, TLS info
- `curl -I` for header-only checks
- `wget -S` as alternative

## Common Failure Patterns

- 401/403: auth/permissions
- 404: wrong routing or path
- 429: rate limiting (respect Retry-After)
- 502/503/504: proxy or upstream failures (see Module 14)
