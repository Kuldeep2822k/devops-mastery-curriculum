---
title: Supply Chain Basics (Dependencies, Images, Provenance)
tags:
  - security
  - supply-chain
module: "10"
---

# Supply Chain Basics (Dependencies, Images, Provenance)

## What “Supply Chain” Means Here

Everything you build depends on inputs:

- base images
- dependencies (libraries)
- build tools and CI runners
- artifact registries

If any input is compromised, your output can be compromised.

## Operator-Relevant Controls

- pin versions (avoid floating latest)
- record inputs (lockfiles, manifests)
- scan for known bad patterns (secrets, forbidden tags)
- attach metadata to artifacts (commit SHA, manifest, later SBOM)

## SBOM Mindset (Even Without Tools)

Even if you don’t have SBOM tooling yet, you can practice:

- generating a dependency manifest
- storing it with build artifacts
- using it during incident response (“what version was deployed?”)

## Provenance Mindset (Even Without Signing)

Practice recording:

- source revision
- build environment (CI job)
- artifact identity (digest/version)

## Anti-Patterns

- unpinned dependencies
- `:latest` base images
- rebuilding artifacts without recording inputs
