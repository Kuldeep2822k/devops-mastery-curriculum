---
title: "Lab 01: Rollout and Rollback (kind)"
tags:
  - lab
  - delivery
  - kubernetes
  - rollback
module: "07"
---

# Lab 01 — Rollout and Rollback (kind)

## Goal

Practice safe delivery mechanics in Kubernetes:

- deploy v1 and verify
- deploy v2 that introduces a controlled failure
- detect impact via signals
- rollback and verify recovery
- capture evidence and write runbook notes

## Prereqs

- kind/minikube cluster running
- `kubectl`
- ability to create namespaces

## Setup

Create namespace:

```bash
kubectl create ns mod07 || true
```

Create a baseline service using `hashicorp/http-echo` with a version string:

```bash
cat > deploy.yaml <<'EOF'
apiVersion: apps/v1
kind: Deployment
metadata:
  name: echo
  labels:
    app: echo
spec:
  replicas: 2
  selector:
    matchLabels:
      app: echo
  template:
    metadata:
      labels:
        app: echo
    spec:
      containers:
        - name: echo
          image: hashicorp/http-echo:1.0.0
          args: ["-text=version=v1"]
          ports:
            - containerPort: 5678
          readinessProbe:
            httpGet:
              path: /
              port: 5678
            initialDelaySeconds: 2
            periodSeconds: 5
          livenessProbe:
            httpGet:
              path: /
              port: 5678
            initialDelaySeconds: 5
            periodSeconds: 10
          resources:
            requests:
              cpu: "25m"
              memory: "32Mi"
            limits:
              cpu: "200m"
              memory: "128Mi"
---
apiVersion: v1
kind: Service
metadata:
  name: echo
spec:
  selector:
    app: echo
  ports:
    - name: http
      port: 80
      targetPort: 5678
EOF
kubectl -n mod07 apply -f deploy.yaml
kubectl -n mod07 rollout status deploy/echo
kubectl -n mod07 get pods,svc,ep -o wide
```

## Steps

### 1) Verify Baseline (v1)

Port-forward:

```bash
kubectl -n mod07 port-forward svc/echo 18090:80
```

In another terminal:

```bash
for i in $(seq 1 10); do curl -fsS http://localhost:18090/; echo; done
```

Expected signals:

- responses include `version=v1`
- no errors

Stop port-forward.

### 2) Deploy v2 With a Controlled Failure (Readiness Broken)

Patch readiness probe to a wrong path:

```bash
kubectl -n mod07 patch deploy/echo -p '{"spec":{"template":{"spec":{"containers":[{"name":"echo","readinessProbe":{"httpGet":{"path":"/nope","port":5678}}}]}}}}'
kubectl -n mod07 rollout status deploy/echo || true
```

Observe:

```bash
kubectl -n mod07 get pods
kubectl -n mod07 get ep echo
kubectl -n mod07 get events --sort-by=.lastTimestamp | tail -n 50
kubectl -n mod07 describe pod -l app=echo | sed -n '1,220p'
```

Expected signals:

- pods may run but not Ready
- endpoints empty or reduced
- events show readiness probe failing

### 3) Containment and Rollback

Rollback to previous ReplicaSet revision:

```bash
kubectl -n mod07 rollout undo deploy/echo
kubectl -n mod07 rollout status deploy/echo
```

### 4) Verify Recovery

```bash
kubectl -n mod07 get ep echo
kubectl -n mod07 port-forward svc/echo 18090:80
```

In another terminal:

```bash
for i in $(seq 1 10); do curl -fsS http://localhost:18090/; echo; done
```

Expected signals:

- responses return
- endpoints populated again

Stop port-forward.

## Verify

- rollout history shows at least one revision and a rollback:

```bash
kubectl -n mod07 rollout history deploy/echo
kubectl -n mod07 get pods
kubectl -n mod07 get ep echo
```

## Cleanup

```bash
kubectl delete ns mod07 --wait=true
rm -f deploy.yaml
```

Verify cleanup:

```bash
kubectl get ns mod07 || true
```

## Troubleshooting

### Symptom: rollout undo doesn’t change state

Diagnosis:

- check rollout history and revision numbers

```bash
kubectl -n mod07 rollout history deploy/echo
```

Fix:

- ensure a revision exists; make another change to create a revision, then rollback

### Symptom: port-forward fails

Diagnosis:

- service endpoints empty

Fix:

- verify endpoints and pod readiness first

## Why This Matters in Production

- Rollback is the fastest safe containment for regressions when rollback is compatible.
- Readiness probe failures are one of the most common “service down” causes after deploy.

## What to Write in a Runbook

- commands to verify rollout status and endpoints
- explicit rollback procedure and triggers
- post-rollback verification window

## Definition of Done

- You can induce a rollout failure, diagnose via endpoints/events, rollback, and verify recovery.
