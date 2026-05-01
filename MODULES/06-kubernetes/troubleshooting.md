---
title: Troubleshooting Guide
tags:
  - troubleshooting
  - kubernetes
module: "06"
---

# Troubleshooting — Module 06 Kubernetes

Use the global framework: [Troubleshooting Framework](../../00-HOW-TO-USE/05-troubleshooting-framework.md)

## Default Debug Playbook (Copyable)

Cluster scope:

```bash
kubectl get pods -A
kubectl get events -A --sort-by=.lastTimestamp | tail -n 80
```

Namespace scope:

```bash
kubectl -n <ns> get deploy,rs,pods,svc,ep,ingress -o wide
kubectl -n <ns> get events --sort-by=.lastTimestamp | tail -n 80
```

Single pod:

```bash
kubectl -n <ns> describe pod <pod>
kubectl -n <ns> logs <pod> --previous || true
kubectl -n <ns> logs <pod> || true
kubectl -n <ns> exec -it <pod> -- sh
```

Service routing:

```bash
kubectl -n <ns> describe svc <svc>
kubectl -n <ns> get ep <svc> -o yaml
kubectl -n <ns> get pods --show-labels
```

DNS check (inside cluster):

```bash
kubectl -n <ns> run dns-debug --rm -it --restart=Never --image=busybox:1.36 -- sh -lc "nslookup kubernetes.default.svc.cluster.local || true"
```

## Symptom → Likely Causes

### CrashLoopBackOff

- app crashes due to config/env missing
- liveness probe killing app
- dependency unavailable

### ImagePullBackOff

- wrong tag
- registry auth / TLS / DNS issues

### Pending

- insufficient resources
- node selector/taint mismatch
- PVC not bound

### Service 503 / No Endpoints

- label mismatch
- readiness failures
- pods not Ready

### RBAC Deny

- service account lacks permission
- wrong kubeconfig context/user
