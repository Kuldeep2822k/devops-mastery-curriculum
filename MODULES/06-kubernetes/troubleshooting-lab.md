---
title: Troubleshooting Lab (Scenarios)
tags:
  - troubleshooting-lab
  - kubernetes
  - oncall
module: "06"
---

# Troubleshooting Lab — Module 06 Kubernetes

## Instructions

- Do 12–20 scenarios.
- Time-box each: 10–20 minutes.
- For each: symptoms, constraints, hints, diagnosis commands, root cause, fix, verification, prevention.

## Scenarios

### Scenario 01 — CrashLoopBackOff (Bad Config)

- Symptoms: pod CrashLoopBackOff; logs show config error.
- Constraints: one restart budget after fix.
- Hints: check env/config maps and logs.
- Diagnosis commands:
  - `kubectl -n <ns> get pods`
  - `kubectl -n <ns> describe pod <pod>`
  - `kubectl -n <ns> logs <pod> --previous`
- Root cause: missing env var or invalid config.
- Fix: correct config; rollout new pod.
- Verification: pod Ready; errors stop.
- Prevention: config validation gate in CI; startup checks.

### Scenario 02 — ImagePullBackOff

- Symptoms: pod stuck; events show image pull failure.
- Constraints: do not change cluster; fix workload config.
- Hints: wrong tag vs auth/DNS.
- Diagnosis commands:
  - `kubectl -n <ns> describe pod <pod>`
  - `kubectl -n <ns> get events --sort-by=.lastTimestamp | tail -n 50`
- Root cause: wrong image tag or registry auth issue.
- Fix: correct image reference; ensure imagePullSecrets if required.
- Verification: pod pulls image and becomes Running/Ready.
- Prevention: pin digests; preflight check image exists.

### Scenario 03 — Pending (Insufficient Resources)

- Symptoms: pod Pending; events mention insufficient cpu/memory.
- Constraints: cannot add nodes (local lab).
- Hints: reduce requests or scale down.
- Diagnosis commands:
  - `kubectl describe pod <pod> -n <ns>`
  - `kubectl get nodes`
- Root cause: requests exceed node capacity.
- Fix: reduce requests; or reduce replicas; or free resources.
- Verification: pod scheduled and starts.
- Prevention: capacity planning; right-size requests.

### Scenario 04 — Pending (Node Selector / Taints)

- Symptoms: pod Pending; events mention node selector/taints.
- Constraints: do not remove taints globally.
- Hints: selectors and tolerations must match.
- Diagnosis commands:
  - `kubectl describe pod <pod> -n <ns>`
  - `kubectl describe node <node> | sed -n '1,120p'`
- Root cause: incompatible scheduling constraints.
- Fix: correct nodeSelector/affinity/tolerations.
- Verification: pod scheduled.
- Prevention: document scheduling policy; test manifests.

### Scenario 05 — OOMKilled

- Symptoms: container restarts; last state shows OOMKilled.
- Constraints: do not “just raise limits” without evidence.
- Hints: check memory usage patterns and limits.
- Diagnosis commands:
  - `kubectl describe pod <pod> -n <ns>`
  - `kubectl logs <pod> -n <ns> --previous`
- Root cause: memory limit too low or memory leak.
- Fix: adjust limit with justification; reduce memory usage; tune requests.
- Verification: pod stays Running; no OOMKilled events.
- Prevention: memory telemetry; load test; leak detection.

### Scenario 06 — Readiness Probe Failure

- Symptoms: pod running but not Ready; Service endpoints empty.
- Constraints: no manual pod edits.
- Hints: probe path/port/timeout mismatch.
- Diagnosis commands:
  - `kubectl describe pod <pod> -n <ns>`
  - `kubectl get ep -n <ns> <svc>`
- Root cause: readiness probe wrong or dependency not ready.
- Fix: correct probe or add startup probe; tune delays.
- Verification: pod Ready; endpoints populated.
- Prevention: define probe semantics; test locally.

### Scenario 07 — Liveness Probe Killing Healthy Startup

- Symptoms: restart loop; logs show normal startup but killed by liveness.
- Constraints: minimize downtime.
- Hints: add startup probe or increase liveness delay.
- Diagnosis commands:
  - `kubectl describe pod <pod> -n <ns>`
  - events show liveness failures
- Root cause: liveness too aggressive.
- Fix: add startup probe / tune liveness.
- Verification: pod stabilizes and Ready.
- Prevention: standard probe profiles; benchmark startup.

### Scenario 08 — Service 503 (No Endpoints)

- Symptoms: ingress/service returns 503; endpoints empty.
- Constraints: do not restart ingress.
- Hints: label mismatch or readiness.
- Diagnosis commands:
  - `kubectl get svc,ep -n <ns>`
  - `kubectl get pods -n <ns> --show-labels`
  - `kubectl describe svc <svc> -n <ns>`
- Root cause: selector mismatch or pods not Ready.
- Fix: align labels/selectors; fix readiness.
- Verification: endpoints populated; 200 response.
- Prevention: admission checks; lint manifests.

### Scenario 09 — Ingress Misroutes (Wrong Host/Path)

- Symptoms: 404 or wrong backend.
- Constraints: must change only ingress config.
- Hints: rule mismatch.
- Diagnosis commands:
  - `kubectl describe ingress <ing> -n <ns>`
  - check service target and ports
- Root cause: wrong host/path/service/port in ingress.
- Fix: correct ingress rules.
- Verification: correct backend serves traffic.
- Prevention: test ingress rules in staging; smoke checks.

### Scenario 10 — DNS Failure Inside Cluster

- Symptoms: pods can’t resolve service names.
- Constraints: don’t assume coreDNS broken immediately.
- Hints: check service exists and correct FQDN.
- Diagnosis commands:
  - `kubectl -n <ns> get svc`
  - run dns-debug pod and `nslookup`
  - `kubectl -n kube-system get pods`
- Root cause: wrong name, missing service, or DNS component unhealthy.
- Fix: correct name or restore DNS components.
- Verification: nslookup succeeds and calls succeed.
- Prevention: include DNS tests in debug playbook.

### Scenario 11 — RBAC Deny (kubectl forbidden)

- Symptoms: “forbidden” errors.
- Constraints: must not grant cluster-admin.
- Hints: inspect Role/RoleBinding and service account.
- Diagnosis commands:
  - `kubectl auth can-i get pods -n <ns> --as=system:serviceaccount:<ns>:<sa> || true`
  - `kubectl get role,rolebinding -n <ns>`
- Root cause: missing permission in role binding.
- Fix: add least-privilege rule; bind to correct service account.
- Verification: can-i returns yes; operation succeeds.
- Prevention: RBAC templates; test can-i checks in CI.

### Scenario 12 — CrashLoopBackOff (Missing File)

- Symptoms: app fails because file not found.
- Constraints: must fix via ConfigMap/Secret/volume, not exec edits.
- Hints: mount paths and working directory.
- Diagnosis commands:
  - `kubectl describe pod <pod> -n <ns>`
  - `kubectl logs <pod> -n <ns> --previous`
- Root cause: missing mounted config or wrong path.
- Fix: correct volume mounts and paths.
- Verification: pod starts and Ready.
- Prevention: startup validation; CI deploy test.

### Scenario 13 — Pending PVC Blocks Pod

- Symptoms: pod Pending; events mention PVC.
- Constraints: local cluster storage may be limited.
- Hints: check StorageClass and PVC.
- Diagnosis commands:
  - `kubectl get pvc,pv -n <ns>`
  - `kubectl describe pvc <pvc> -n <ns>`
  - `kubectl get storageclass`
- Root cause: no StorageClass or provisioner mismatch.
- Fix: use correct StorageClass or adjust PVC.
- Verification: PVC Bound; pod scheduled.
- Prevention: document storage assumptions; validate in staging.

### Scenario 14 — Service Port Mismatch

- Symptoms: endpoints exist but traffic fails.
- Constraints: fix via Service/Deployment config only.
- Hints: targetPort mismatch with containerPort.
- Diagnosis commands:
  - `kubectl describe svc <svc> -n <ns>`
  - `kubectl describe pod <pod> -n <ns>`
- Root cause: Service routes to wrong targetPort.
- Fix: correct Service port mapping.
- Verification: request succeeds.
- Prevention: standard port naming; tests.

## Time-Boxed On-Call Drill (30–60 minutes)

Pick a realistic combo: Service 503 + pods restarting.

- constraints: no deleting pods without evidence, no cluster-admin grants
- deliverables:
  - incident timeline with commands and reasons
  - containment decision (rollback vs forward-fix)
  - root cause and fix
  - verification window and prevention tasks
