---
title: Container Mental Model (What Actually Happens)
tags:
  - containers
  - mental-model
module: "05"
---

# Container Mental Model (What Actually Happens)

## Container ≠ VM

A container is a process with:

- filesystem view (image + writable layer)
- namespace isolation (PID, net, mount, user)
- resource controls (cgroups)

Operator implication:

- container failures are often “just Linux” failures
- you debug with process, filesystem, and network tools, plus container tooling

## Image vs Container

- image: immutable template (layers)
- container: a running instance with a writable layer

Anti-pattern:

- “patching” a running container and expecting it to persist

Production pattern:

- rebuild image, redeploy new container

## Namespaces and “Why localhost is confusing”

Inside a container:

- `localhost` is the container’s network namespace, not the host

Common failure:

- app tries to connect to `localhost:5432` expecting host DB, but no DB in the container

## Volumes and Persistence

Volumes exist because:

- container writable layers are ephemeral

Operator habit:

- know which data is ephemeral vs persistent
- verify volume mounts and ownership

## Resource Limits Are Part of Design

Without limits, a container can:

- consume host memory and cause host instability

With limits, you can get:

- OOMKilled, throttling, performance cliffs

Operator goal:

- set reasonable limits, then observe and tune

## The First 5 Minutes of Container Debugging

1) Is it running? `docker ps -a`  
2) Why did it exit? `docker logs` and `docker inspect`  
3) Is it listening? check exposed ports and container port binding  
4) Is it healthy? probe endpoint from host and from inside container  
5) Are there resource/permission/DNS issues? check stats, mounts, resolv.conf  
