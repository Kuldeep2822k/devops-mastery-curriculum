---
title: "Deep Dive 02: Debuggability vs Minimalism (The Debug Image Strategy)"
tags:
  - containers
  - deep-dive
  - tradeoffs
module: "05"
---

# Deep Dive 02 — Debuggability vs Minimalism (The Debug Image Strategy)

## The Tradeoff

Minimal images improve:

- security surface area
- pull time
- cold start behavior

But reduce:

- ability to debug inside the container (missing shell, curl, dig, ps)

## Common Patterns

### Pattern A: Minimal Production Image + Separate Debug Image

- prod image: minimal, non-root, only what you need
- debug image: includes tools, used only in controlled environments

Benefit:

- keeps production surface small while preserving debug capability

### Pattern B: Debug Sidecar / Ephemeral Debug Container (Kubernetes later)

Add debug tooling without changing production image.

Benefit:

- avoids shipping debug tools in prod image

### Pattern C: “Include Tools in Prod Image”

Sometimes justified when:

- platform constraints prevent debug containers
- operational needs require on-box tooling

Risk:

- larger attack surface
- inconsistent tool versions across services

## Staff-Level Guidance

Default:

- minimal production images + standardized debugging strategy

Write the choice as an ADR because:

- it affects incident response
- it affects security posture
- it affects build and deploy complexity
