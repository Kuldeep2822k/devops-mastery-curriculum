---
title: 'Kubernetes Failures'
tags:
  - catalog
  - kubernetes
  - incident
---

# Kubernetes Failures — Troubleshooting Scenarios

Use this when workloads are failing to schedule, start, or route traffic correctly.

## Scenarios

### Scenario 01 — CrashLoopBackOff

- Symptoms: Pod restarts; readiness never becomes ready.
- Diagnosis commands:
  - `kubectl -n <ns> describe pod <pod>`
  - `kubectl -n <ns> logs <pod> --previous --tail=200`
  - `kubectl -n <ns> get events --sort-by=.lastTimestamp | tail -n 50`
- Root cause: Bad config, missing secret, app crash, startup dependency down.
- Fix: Roll back; fix config; add backoff; stabilize.
- Verify: Pod reaches Ready and stays stable.
- Prevention: Startup checks; config validation; canary deploy.

### Scenario 02 — ImagePullBackOff

- Symptoms: Pod stuck pulling image.
- Diagnosis commands:
  - `kubectl -n <ns> describe pod <pod> | sed -n '1,200p'`
  - `kubectl -n <ns> get secret | grep -i pull || true`
- Root cause: Wrong tag, missing pull secret, registry outage/rate limit.
- Fix: Correct image reference; add pull secret; mirror.
- Verify: Image pulls; container starts.
- Prevention: Pin digests; registry caching.

### Scenario 03 — Pending (unschedulable)

- Symptoms: Pod never scheduled.
- Diagnosis commands:
  - `kubectl -n <ns> describe pod <pod> | sed -n '1,220p'`
  - `kubectl get nodes -o wide`
  - `kubectl describe node <node> | sed -n '1,220p'`
- Root cause: Resource requests too high, taints/tolerations, affinity, quota.
- Fix: Adjust requests/limits; add tolerations; scale nodes (if allowed).
- Verify: Pod scheduled and running.
- Prevention: Capacity planning; quota + sizing defaults.

### Scenario 04 — OOMKilled

- Symptoms: Container restarts; `OOMKilled` in status.
- Diagnosis commands:
  - `kubectl -n <ns> describe pod <pod> | grep -i -E 'OOMKilled|Last State' -n || true`
  - `kubectl -n <ns> top pod <pod> || true`
- Root cause: Memory leak or undersized limits.
- Fix: Increase limit temporarily; reduce concurrency; fix app memory.
- Verify: No more OOM kills; memory stable.
- Prevention: Memory alerts; load tests.

### Scenario 05 — Readiness probe failures cause 503

- Symptoms: Service returns 503; endpoints empty.
- Diagnosis commands:
  - `kubectl -n <ns> get endpoints <svc> -o yaml | head -n 80`
  - `kubectl -n <ns> describe pod <pod> | grep -i readiness -n || true`
- Root cause: Probe misconfigured, wrong port/path, app slow start.
- Fix: Fix probe; separate liveness vs readiness; add startupProbe.
- Verify: Endpoints populated; traffic succeeds.
- Prevention: Probe guidelines and review.

### Scenario 06 — Service routing broken (selector mismatch)

- Symptoms: Service has no endpoints.
- Diagnosis commands:
  - `kubectl -n <ns> get svc <svc> -o yaml | sed -n '1,120p'`
  - `kubectl -n <ns> get pods --show-labels`
- Root cause: Selector doesn’t match pod labels.
- Fix: Align labels/selectors; redeploy safely.
- Verify: Endpoints exist; requests succeed.
- Prevention: Label conventions; CI validation.

### Scenario 07 — DNS failures in cluster

- Symptoms: Pods cannot resolve names; intermittent.
- Diagnosis commands:
  - `kubectl -n kube-system get pods -l k8s-app=kube-dns -o wide`
  - `kubectl -n <ns> exec <pod> -- sh -lc 'cat /etc/resolv.conf; nslookup kubernetes.default || true'`
- Root cause: CoreDNS down, upstream resolver issue, network policy.
- Fix: Restore CoreDNS; fix upstream; validate policies.
- Verify: Name resolution works.
- Prevention: CoreDNS alerts and runbook.
