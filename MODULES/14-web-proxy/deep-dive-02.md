---
title: "Deep Dive 02: Security Edges (Host Header, Auth Forwarding)"
tags:
  - web-proxy
  - deep-dive
  - security
module: "14"
---

# Deep Dive 02 — Security Edges (Host Header, Auth Forwarding)

## Host Header Injection

If upstream trusts Host for:

- routing
- auth callbacks
- redirect URLs

then a spoofed Host can cause:

- open redirects
- auth confusion
- cache poisoning

Mitigation:

- validate allowed hosts at proxy
- set and normalize Host header intentionally

## Auth Header Forwarding

Avoid:

- forwarding Authorization headers to unintended upstreams

Patterns:

- terminate auth at proxy and forward identity claims safely
- use separate internal headers and strip external inputs

## Client IP Trust

Only trust X-Forwarded-For when requests come from trusted proxy layers.

## Staff-Level Guidance

- define proxy security policy in an ADR (headers, auth, allowed hosts).
