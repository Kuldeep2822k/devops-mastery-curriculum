---
title: "Lab 02: Canary and Promotion (Local-First)"
tags:
  - lab
  - delivery
  - canary
module: "07"
---

# Lab 02 — Canary and Promotion (Local-First)

## Goal

Practice a canary-style rollout and promotion decision using basic primitives:

- run stable and canary side-by-side
- send test traffic and observe signals
- promote canary or rollback
- record artifact identity and rollout decision

This lab uses coarse traffic control (manual testing + replica ratios). Later modules/projects introduce more advanced traffic splitting.

## Prereqs

- local Kubernetes cluster running
- `kubectl`

## Setup

Create namespace:

```bash
kubectl create ns mod07-canary || true
```

Create stable deployment + service:

```bash
cat > canary.yaml <<'EOF'
apiVersion: apps/v1
kind: Deployment
metadata:
  name: echo-stable
  labels:
    app: echo
    track: stable
spec:
  replicas: 3
  selector:
    matchLabels:
      app: echo
      track: stable
  template:
    metadata:
      labels:
        app: echo
        track: stable
    spec:
      containers:
        - name: echo
          image: hashicorp/http-echo:1.0.0
          args: ["-text=track=stable version=v1"]
          ports:
            - containerPort: 5678
          readinessProbe:
            httpGet:
              path: /
              port: 5678
            initialDelaySeconds: 2
            periodSeconds: 5
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: echo-canary
  labels:
    app: echo
    track: canary
spec:
  replicas: 1
  selector:
    matchLabels:
      app: echo
      track: canary
  template:
    metadata:
      labels:
        app: echo
        track: canary
    spec:
      containers:
        - name: echo
          image: hashicorp/http-echo:1.0.0
          args: ["-text=track=canary version=v2"]
          ports:
            - containerPort: 5678
          readinessProbe:
            httpGet:
              path: /
              port: 5678
            initialDelaySeconds: 2
            periodSeconds: 5
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
kubectl -n mod07-canary apply -f canary.yaml
kubectl -n mod07-canary rollout status deploy/echo-stable
kubectl -n mod07-canary rollout status deploy/echo-canary
kubectl -n mod07-canary get pods -o wide --show-labels
kubectl -n mod07-canary get svc,ep
```

## Steps

### 1) Verify Both Tracks Are Serving

Port-forward:

```bash
kubectl -n mod07-canary port-forward svc/echo 18091:80
```

In another terminal, sample responses:

```bash
for i in $(seq 1 30); do curl -fsS http://localhost:18091/; echo; done | sort | uniq -c
```

Expected signal:

- both stable and canary responses appear (ratio roughly 3:1)

Stop port-forward.

### 2) Break Canary and Observe Signal (Simulate Bad Release)

Break canary readiness probe:

```bash
kubectl -n mod07-canary patch deploy/echo-canary -p '{"spec":{"template":{"spec":{"containers":[{"name":"echo","readinessProbe":{"httpGet":{"path":"/nope","port":5678}}}]}}}}'
kubectl -n mod07-canary rollout status deploy/echo-canary || true
```

Observe:

```bash
kubectl -n mod07-canary get pods
kubectl -n mod07-canary get ep echo
kubectl -n mod07-canary get events --sort-by=.lastTimestamp | tail -n 50
```

Expected signal:

- canary pods not Ready → fewer canary endpoints; stable still serves

### 3) Rollback Canary (Containment)

Rollback canary to previous revision:

```bash
kubectl -n mod07-canary rollout undo deploy/echo-canary
kubectl -n mod07-canary rollout status deploy/echo-canary
```

Verify endpoints:

```bash
kubectl -n mod07-canary get ep echo
```

### 4) Promote Canary (When Healthy)

Promotion means: scale stable down and scale canary up (coarse promotion).

```bash
kubectl -n mod07-canary scale deploy/echo-stable --replicas=0
kubectl -n mod07-canary scale deploy/echo-canary --replicas=4
kubectl -n mod07-canary rollout status deploy/echo-canary
kubectl -n mod07-canary get pods --show-labels
```

Verify responses:

```bash
kubectl -n mod07-canary port-forward svc/echo 18091:80
```

In another terminal:

```bash
for i in $(seq 1 20); do curl -fsS http://localhost:18091/; echo; done | sort | uniq -c
```

Expected signal:

- only canary responses remain (now promoted)

Stop port-forward.

## Verify

- only one track serving after promotion
- endpoints present and stable

```bash
kubectl -n mod07-canary get deploy
kubectl -n mod07-canary get ep echo
```

## Cleanup

```bash
kubectl delete ns mod07-canary --wait=true
rm -f canary.yaml
```

## Troubleshooting

### Symptom: responses don’t show both tracks

Diagnosis:

- endpoints may be skewed; check readiness and labels

```bash
kubectl -n mod07-canary get pods --show-labels
kubectl -n mod07-canary get ep echo -o yaml
```

Fix:

- ensure both deployments are Ready and selected by Service selector `app=echo`.

## Why This Matters in Production

- Canary reduces blast radius but only works with good signals and explicit promotion/rollback decisions.
- Even without advanced traffic splitting, you can practice the operational decision process.

## What to Write in a Runbook

- canary evaluation checklist (signals, windows, rollback triggers)
- promotion steps and verification
- rollback steps and verification

## Definition of Done

- You can run stable+canary, detect a canary failure via readiness/endpoints, rollback, and promote intentionally.
