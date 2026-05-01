---
title: Workloads, Labels, and Rollouts
tags:
  - kubernetes
  - workloads
  - labels
module: "06"
---

# Workloads, Labels, and Rollouts

## Core Workload Primitives (Operator View)

- Pod: a group of containers that share network namespace and volumes.
- Deployment: declarative rollout controller for stateless workloads.
- ReplicaSet: manages a set of identical pod replicas (usually owned by Deployment).
- StatefulSet: stable identity and storage semantics (not “better Deployment”).
- DaemonSet: one pod per node (agents, log collectors).
- Job/CronJob: run-to-completion workloads.

Operator habit:

- always identify the controller owning a pod before “fixing” the pod

## Labels and Selectors Are Traffic Routing

Labels are not tags for humans only. They are how:

- Services select endpoints
- Deployments manage pod sets
- Network policies target pods

Common outage pattern:

- labels mismatch → Service has no endpoints → 503 at ingress/proxy

Diagnosis commands:

```bash
kubectl get deploy,rs,pods,svc -n <ns> -o wide
kubectl get endpoints -n <ns>
kubectl describe svc <svc> -n <ns>
```

## Rollouts and Safety

Deployments roll out changes gradually (by default) but safety depends on:

- readiness probes (traffic gating)
- maxUnavailable/maxSurge settings
- resource requests (scheduling)

Operator habit:

- watch rollout status and events:

```bash
kubectl rollout status deploy/<name> -n <ns>
kubectl get events -n <ns> --sort-by=.lastTimestamp | tail -n 50
```

## Anti-Patterns

- editing pods directly (impermanent and bypasses controllers)
- “kubectl delete pod” as a fix without root cause
- not verifying Service endpoints after deploy
