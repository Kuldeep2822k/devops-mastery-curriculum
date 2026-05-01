---
title: Docker Setup (Local-First)
tags:
  - setup
  - docker
  - containers
---

# Docker Setup (Local-First)

## Goal

Install and validate a container runtime you can trust for labs: predictable networking, bounded resource usage, and clean cleanup behavior.

## Prereqs

- Sufficient disk space (container images consume space fast)
- Virtualization enabled (if required by your OS)

## Setup

### Install Docker

Install Docker using your OS package manager or official packages. Avoid mixing multiple install methods (it often causes daemon conflicts).

### Verify Docker Daemon and CLI

```bash
docker version
docker info
```

Expected signals:

- Client and Server both show versions
- no “cannot connect to Docker daemon”

### Non-Root Docker (Linux)

If your distro supports it, configure user access to Docker without running everything as root. This has security tradeoffs; understand them:

- Docker group access is effectively root-equivalent on the host.
- Prefer using a dedicated lab machine/VM if you are security-sensitive.

Verify:

```bash
docker run --rm hello-world
```

Expected signals:

- container runs and exits successfully

### Resource Limits (Recommended)

If using Docker Desktop, set explicit limits:

- CPU cores
- memory
- disk image size

Reason:

- prevents your laptop from becoming the incident

### Logging Driver Awareness

Know your logging mode:

- default json-file logs can fill disk
- journald integration changes log retrieval commands

Verify:

```bash
docker info | sed -n '1,120p'
```

Expected signals:

- you can locate “Logging Driver”

## Operational Patterns You Will Use Later

### Inspect and Debug

```bash
docker ps
docker logs <container>
docker inspect <container>
docker exec -it <container> sh
```

### Bound Resource Usage

```bash
docker run --rm --memory 256m --cpus 1 alpine:3 sh -c "echo ok; sleep 1"
```

### Cleanup Discipline

Verify what exists:

```bash
docker ps -a
docker images
docker volume ls
docker network ls
```

Safe cleanup pattern (be careful with global prunes):

- prefer deleting only resources created by a lab (named with a lab prefix)
- only use global prune when you understand the blast radius

## Troubleshooting

### Symptom: cannot connect to daemon

Diagnosis:

- is the daemon running?
- do you have permissions?

Fix:

- start the daemon via your OS service manager
- ensure your user is permitted (or use Docker Desktop)

### Symptom: pulls are slow or fail

Diagnosis:

- check DNS and proxy settings
- verify registry reachability

Fix:

- configure proxy settings if required
- use a local registry later for speed and reliability

### Symptom: disk fills up

Diagnosis:

- `docker system df`
- `df -h`

Fix:

- delete unused images/volumes
- reduce log growth; bound image cache; increase Docker disk image size if appropriate

## Why This Matters in Production

- Container runtime reliability issues look like “app bugs” but are host issues.
- Unbounded logs and images cause disk exhaustion outages.
- Debugging containers quickly is core to reducing MTTR.
