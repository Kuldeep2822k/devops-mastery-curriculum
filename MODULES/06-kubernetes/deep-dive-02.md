---
title: "Deep Dive 02: Debugging Under Pressure (Events-First Discipline)"
tags:
  - kubernetes
  - deep-dive
  - debugging
module: "06"
---

# Deep Dive 02 — Debugging Under Pressure (Events-First Discipline)

## Why Kubernetes Debugging Feels Hard

Kubernetes failures are layered:

- app issues (crash, config, dependencies)
- container runtime issues (image pulls, filesystem)
- cluster issues (scheduling, node pressure, DNS)
- routing issues (Service endpoints, ingress)
- identity issues (RBAC denies)

The fix is not “know everything”. The fix is a disciplined playbook.

## Events-First Playbook (Repeatable)

1) Identify scope:

```bash
kubectl get pods -A
```

2) Check events (fastest truth for many failures):

```bash
kubectl get events -A --sort-by=.lastTimestamp | tail -n 80
```

3) Describe failing objects:

```bash
kubectl describe pod <pod> -n <ns>
kubectl describe deploy <deploy> -n <ns>
kubectl describe svc <svc> -n <ns>
```

4) Logs:

```bash
kubectl logs <pod> -n <ns> --previous
kubectl logs <pod> -n <ns>
```

5) Exec and prove runtime state:

```bash
kubectl exec -it <pod> -n <ns> -- sh
```

6) Networking checks:

- DNS resolution from inside the pod
- service endpoints and selectors

## Anti-Patterns That Increase MTTR

- deleting pods before capturing logs/events
- editing live pods instead of controllers
- assuming “Kubernetes bug” without checking labels/endpoints
- restarting ingress controller as first move

## Staff-Level Behavior

- write down hypotheses and update them as evidence changes
- contain blast radius (rollback, reduce traffic) before deep investigation when impact is high
- document the path from symptoms to signals to root cause in a runbook
