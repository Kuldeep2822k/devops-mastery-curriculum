---
title: Images, Layers, Tags, and Digests
tags:
  - containers
  - images
  - supply-chain
module: "05"
---

# Images, Layers, Tags, and Digests

## Layers and Caching

Dockerfiles build in layers.

Implications:

- changing earlier layers invalidates later cache
- ordering Dockerfile steps impacts build speed

Operator habit:

- put stable steps early (base image, system deps)
- put frequently-changing app code later

## Tags Are Pointers, Not Identity

Tags like `:latest` are mutable pointers.

Failure mode:

- “same tag” points to different content over time
- rollback becomes ambiguous

Production-safe approach:

- promote by digest (immutable)
- use tags as human-friendly labels, not the source of truth

## Digests Are Immutable Identity

An image digest (sha256) identifies exact content.

Operational benefits:

- reproducible deploys
- safe rollback targets
- reliable provenance mapping

## Minimal Image Strategy

Tradeoffs:

- slim images reduce attack surface and pull time
- too-slim images reduce debuggability

Staff-level approach:

- use slim in production
- keep a debug image variant or sidecar tooling approach for incident response

## Don’t Bake Secrets into Images

Anti-pattern:

- copying secrets into image layers
- using build args for secrets without protections

Safer patterns:

- inject secrets at runtime (environment/secret store)
- ensure CI logs never print secrets
