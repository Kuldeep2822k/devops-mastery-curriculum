---
title: Probes, Resources, and Scheduling
tags:
  - kubernetes
  - probes
  - resources
  - scheduling
module: "06"
---

# Probes, Resources, and Scheduling

## Probes (Liveness vs Readiness vs Startup)

- readiness: “can this pod receive traffic?”
- liveness: “should this container be restarted?”
- startup: “give me time to start before liveness kills me”

Common outages:

- readiness too strict → no endpoints → 503
- liveness too strict → crash loop due to aggressive restarts
- missing startup probe on slow start → liveness kills app before it stabilizes

Operator habit:

- inspect probe config and events:

```bash
kubectl describe pod <pod> -n <ns>
kubectl get events -n <ns> --sort-by=.lastTimestamp | tail -n 50
```

## Resources: Requests vs Limits

- request: scheduling guarantee and capacity planning input
- limit: maximum allowed; exceeding memory limit triggers OOMKill

Failure patterns:

- no requests → scheduler packs pods tightly → noisy neighbor and instability
- too-low memory limits → OOMKilled
- too-low CPU limits → throttling and latency spikes

## Scheduling and Pending Pods

Pods can be Pending because:

- insufficient CPU/memory on nodes
- node selectors/taints/tolerations mismatch
- PVC not bound
- image pull issues can appear after scheduling (different state)

Diagnosis:

```bash
kubectl describe pod <pod> -n <ns>
kubectl get nodes
kubectl top nodes 2>/dev/null || true
```

## Anti-Patterns

- setting limits without measuring
- ignoring requests and then blaming Kubernetes for scheduling
- treating OOMKilled as “random crash”
