---
title: Runbook Template (Module 06)
tags:
  - runbook
  - template
  - kubernetes
module: "06"
---

# Runbook Template — Module 06 Kubernetes

## Title

Kubernetes — <Symptom (Service 503 / CrashLoopBackOff / ImagePullBackOff / Pending)>

## Impact

- User impact:
- Scope (namespace, service, cluster):
- Severity:

## Safety and Preconditions

- Capture evidence before restarts/rollbacks.
- Prefer fixing controllers (Deployment) over pods.
- No cluster-admin by default; use least privilege.

## Quick Triage (10 minutes)

```bash
kubectl get pods -A
kubectl get events -A --sort-by=.lastTimestamp | tail -n 80
kubectl -n <ns> get deploy,rs,pods,svc,ep,ingress -o wide
kubectl -n <ns> get events --sort-by=.lastTimestamp | tail -n 80
```

## Diagnosis (Playbook)

### Pod-Level

```bash
kubectl -n <ns> describe pod <pod>
kubectl -n <ns> logs <pod> --previous || true
kubectl -n <ns> logs <pod> || true
kubectl -n <ns> exec -it <pod> -- sh
```

### Service-Level

```bash
kubectl -n <ns> describe svc <svc>
kubectl -n <ns> get ep <svc> -o yaml
kubectl -n <ns> get pods --show-labels
```

### DNS Check

```bash
kubectl -n <ns> run dns-debug --rm -it --restart=Never --image=busybox:1.36 -- sh -lc "nslookup <svc> || true"
```

### RBAC Check

```bash
kubectl auth can-i get pods -n <ns> --as=system:serviceaccount:<ns>:<sa> || true
```

## Containment

- rollback deployment: `kubectl -n <ns> rollout undo deploy/<name>`
- reduce blast radius (scale down broken component)

## Fix

- image reference fix
- label/selector fix
- probe tuning (startup/readiness/liveness)
- resource tuning (requests/limits)
- RBAC least privilege binding

## Verification

- rollout status green
- endpoints populated
- probe readiness stable
- user-facing checks (curl / synthetic)

## Post-Fix Monitoring

- observe events and restart counts for 10–30 minutes
- watch error rates/latency if available

## Prevention / Follow-Ups

- add manifest linting and policy checks
- add smoke tests per deploy
- write/upgrade runbook links in alerts
