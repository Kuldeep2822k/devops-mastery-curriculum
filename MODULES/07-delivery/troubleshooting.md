---
title: Troubleshooting Guide
tags:
  - troubleshooting
  - delivery
module: "07"
---

# Troubleshooting — Module 07 Delivery

## Fast Classification (Delivery Incidents)

- Wrong version deployed?
- Rollout stuck or failing probes?
- Service routing broken (endpoints empty)?
- Canary unhealthy?
- Migration/config incompatibility?

## Core Commands (Kubernetes)

```bash
kubectl -n <ns> rollout status deploy/<name>
kubectl -n <ns> rollout history deploy/<name>
kubectl -n <ns> get pods,svc,ep -o wide
kubectl -n <ns> get events --sort-by=.lastTimestamp | tail -n 80
kubectl -n <ns> describe pod <pod>
kubectl -n <ns> logs <pod> --previous || true
```

## Rollback Commands (Baseline)

```bash
kubectl -n <ns> rollout undo deploy/<name>
```

Before rollback:

- capture current events and logs
- record current image identity:

```bash
kubectl -n <ns> get deploy/<name> -o jsonpath='{.spec.template.spec.containers[*].image}{"\n"}'
```

## Symptom Patterns

### Service 503 After Deploy

Likely:

- readiness failures
- label mismatch → endpoints empty

Diagnosis:

- check endpoints and pod readiness

### Rollout Stuck

Likely:

- probe failure
- image pull issue
- insufficient resources

Diagnosis:

- events-first
- describe pod

### Rollback Didn’t Help

Likely:

- schema/config incompatibility
- external dependency issue
- traffic still routed to broken component

Diagnosis:

- verify actual artifact deployed after rollback
- verify config and dependencies

## Safe Delivery Habits

- one change at a time
- explicit verification window
- define rollback triggers before deploy
