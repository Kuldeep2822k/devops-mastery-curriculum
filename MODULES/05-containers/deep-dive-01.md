---
title: "Deep Dive 01: Production Container Failure Patterns"
tags:
  - containers
  - deep-dive
  - failure-modes
module: "05"
---

# Deep Dive 01 — Production Container Failure Patterns

## Image Pull and Registry Failures

Symptoms:

- ImagePullBackOff (in Kubernetes later)
- docker pull fails (auth, DNS, TLS)

Common root causes:

- expired registry credentials
- DNS resolution failures
- rate limiting
- wrong tag or missing image

Prevention:

- use a reliable registry and credential rotation
- pin images by digest
- cache commonly used images where appropriate

## Entrypoint and Command Problems

Symptoms:

- container exits immediately
- “exec format error”
- “no such file or directory”

Root causes:

- wrong architecture image (arm vs amd64)
- missing executable due to wrong COPY paths
- wrong working directory or relative paths

Prevention:

- CI smoke tests that run the container and hit a health endpoint
- multi-arch build strategy with explicit targets

## Permissions and Filesystem Issues

Symptoms:

- cannot write to /data
- “permission denied” on startup

Root causes:

- running as non-root but volume mounted with root ownership
- read-only filesystem without writable dirs for app needs

Prevention:

- run as non-root but design writable paths explicitly
- verify mounts and ownership in CI

## Resource Limits and OOM

Symptoms:

- container killed, exit code 137
- restarts and partial logs

Root causes:

- memory leak
- too-low memory limits

Prevention:

- tune limits with evidence
- add memory usage telemetry and alerts

## “It Works in Docker, Fails in Orchestrator”

Common reasons:

- different network environment and DNS
- different health probes and timeouts
- different volume permissions model

Prevention:

- consistent health endpoint and startup behavior
- run a local orchestrator (kind) later and practice the same patterns
