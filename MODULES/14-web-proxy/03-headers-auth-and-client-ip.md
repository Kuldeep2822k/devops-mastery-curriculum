---
title: Headers, Auth, and Client IP
tags:
  - web-proxy
  - headers
  - security
module: "14"
---

# Headers, Auth, and Client IP

## Header Forwarding Is Security-Sensitive

Common headers:

- Host
- X-Forwarded-For
- X-Forwarded-Proto
- X-Request-Id

Risks:

- host header injection if upstream trusts Host blindly
- spoofed client IP if you trust X-Forwarded-For from untrusted sources

## Auth at the Proxy

Proxy can enforce:

- basic auth
- OAuth/OIDC integration (via upstream auth services)
- mTLS (later modules)

Operator habit:

- ensure upstream knows whether proxy already authenticated the request
- avoid forwarding auth headers to unintended upstreams

## Real Client IP

To preserve client IP:

- proxy must set forwarded headers
- app must be configured to trust proxy and parse them correctly

Failure pattern:

- rate limiting and audit logs break because everything appears to come from proxy IP
