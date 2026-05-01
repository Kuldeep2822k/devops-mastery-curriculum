---
title: Reverse Proxy Mental Model
tags:
  - web-proxy
  - reverse-proxy
module: "14"
---

# Reverse Proxy Mental Model

## What a Reverse Proxy Does

- accepts client connections
- forwards requests to upstream services
- applies policy (routing, auth, rate limits, caching)
- terminates TLS (often)

## Why Proxies Exist Operationally

- centralize security controls
- reduce complexity in application services
- provide traffic shaping and resilience features

## Common Layers to Debug

1) client → proxy connectivity  
2) proxy routing and host/path rules  
3) proxy → upstream connectivity  
4) upstream health and response behavior  
5) headers and protocol upgrades  

## “It Works Locally” Proxy Failure Pattern

- app is healthy when called directly
- proxy returns 502/504 due to timeout, DNS, or wrong upstream address

Operator habit:

- test direct upstream and through proxy and compare.
