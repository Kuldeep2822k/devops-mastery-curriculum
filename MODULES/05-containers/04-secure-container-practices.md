---
title: Secure Container Practices (Safe Defaults)
tags:
  - containers
  - security
module: "05"
---

# Secure Container Practices (Safe Defaults)

## Goals

- reduce attack surface
- reduce blast radius if compromised
- maintain operability for debugging and incident response

## Run as Non-Root

Default: run apps as a non-root user inside container.

Failure mode:

- permission issues on mounted volumes

Operator habit:

- ensure mounted paths are writable by the container user (or use fsGroup in Kubernetes later)

## Minimal Permissions and Capabilities

Avoid privileged containers unless necessary.

Tradeoff:

- too strict → breaks legitimate operations (binding low ports, writing to certain paths)

## Immutable Infrastructure Mindset

Do:

- rebuild and redeploy images for changes

Don’t:

- exec into production containers and “hot patch” as the normal workflow

## Supply Chain Basics

Start building habits:

- pin base images (avoid floating latest)
- track SBOM generation (later modules deepen this)
- promote images by digest for reliability

## Secrets

Rules:

- never bake secrets into images
- never print secrets
- inject secrets at runtime using environment/secret stores

## Anti-Patterns

- using `:latest` in production deployments
- running as root with full privileges for convenience
- copying `.env` files into images
- storing credentials in build args without strict controls
