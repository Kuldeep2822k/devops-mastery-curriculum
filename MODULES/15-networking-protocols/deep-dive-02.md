---
title: "Deep Dive 02: DNS as a Production Dependency"
tags:
  - networking
  - deep-dive
  - dns
module: "15"
---

# Deep Dive 02 — DNS as a Production Dependency

## Why DNS Causes Incidents

- DNS is used everywhere (service discovery, APIs, auth callbacks).
- DNS failures often look like “network down”.
- caching and TTL make failures inconsistent across clients.

## Failure Patterns

- intermittent failures due to resolver timeouts
- stale records due to caching
- split-horizon causing different answers inside/outside network

## Operational Controls

- redundant resolvers
- monitoring query success rate and latency
- lowering TTL before planned cutovers (then raising later)

## Staff-Level Guidance

DNS should have:

- clear ownership
- runbooks for resolver outages
- change control for record updates
