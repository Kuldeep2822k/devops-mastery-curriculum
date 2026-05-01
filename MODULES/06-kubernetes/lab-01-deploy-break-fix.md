---
title: "Lab 01: Deploy + Break + Fix (Image Pull, Labels, Probes, Resources)"
tags:
  - lab
  - kubernetes
  - troubleshooting
module: "06"
---

# Lab 01 — Deploy + Break + Fix (Image Pull, Labels, Probes, Resources)

## Goal

Deploy a simple app to local Kubernetes and practice debugging by breaking:

- ImagePullBackOff (bad image)
- Service endpoints missing (label mismatch)
- probe failures (readiness/liveness)
- OOMKilled (memory limit too low)

## Prereqs

- kind or minikube cluster running
- kubectl configured
- ability to create namespaces

## Setup

Create a namespace:

```bash
kubectl create ns mod06 || true
kubectl -n mod06 get ns mod06
```

Create a baseline deployment + service (uses a stable public image):

```bash
cat > k8s.yaml <<'EOF'
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
  labels:
    app: web
spec:
  replicas: 1
  selector:
    matchLabels:
      app: web
  template:
    metadata:
      labels:
        app: web
    spec:
      containers:
        - name: web
          image: nginx:1.27
          ports:
            - containerPort: 80
          readinessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 2
            periodSeconds: 5
          livenessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 5
            periodSeconds: 10
          resources:
            requests:
              cpu: "50m"
              memory: "64Mi"
            limits:
              cpu: "250m"
              memory: "128Mi"
---
apiVersion: v1
kind: Service
metadata:
  name: web
  labels:
    app: web
spec:
  selector:
    app: web
  ports:
    - name: http
      port: 80
      targetPort: 80
EOF
kubectl -n mod06 apply -f k8s.yaml
```

## Steps

### 1) Verify Baseline Works

```bash
kubectl -n mod06 rollout status deploy/web
kubectl -n mod06 get pods -o wide
kubectl -n mod06 get svc,ep
```

Port-forward and verify:

```bash
kubectl -n mod06 port-forward svc/web 18080:80
```

In a second terminal:

```bash
curl -fsS http://localhost:18080/ | head
```

Expected signals:

- rollout completes
- endpoints not empty
- curl returns nginx HTML

Stop port-forward when done.

### 2) Break: ImagePullBackOff

Patch deployment image to a non-existent tag:

```bash
kubectl -n mod06 set image deploy/web web=nginx:does-not-exist
```

Observe:

```bash
kubectl -n mod06 get pods
kubectl -n mod06 describe pod -l app=web | sed -n '1,160p'
kubectl -n mod06 get events --sort-by=.lastTimestamp | tail -n 50
```

Root cause:

- image tag not found / pull fails.

Fix:

```bash
kubectl -n mod06 set image deploy/web web=nginx:1.27
kubectl -n mod06 rollout status deploy/web
```

### 3) Break: Service Has No Endpoints (Label Mismatch)

Patch pod template label so it no longer matches the Service selector:

```bash
kubectl -n mod06 patch deploy/web -p '{"spec":{"template":{"metadata":{"labels":{"app":"web-broken"}}}}}'
kubectl -n mod06 rollout status deploy/web
```

Observe:

```bash
kubectl -n mod06 get pods --show-labels
kubectl -n mod06 get ep web -o yaml
kubectl -n mod06 describe svc web
```

Expected signal:

- endpoints empty.

Fix (restore label):

```bash
kubectl -n mod06 patch deploy/web -p '{"spec":{"template":{"metadata":{"labels":{"app":"web"}}}}}'
kubectl -n mod06 rollout status deploy/web
kubectl -n mod06 get ep web
```

### 4) Break: Readiness Probe Failure

Patch readiness path to a non-existent path:

```bash
kubectl -n mod06 patch deploy/web -p '{"spec":{"template":{"spec":{"containers":[{"name":"web","readinessProbe":{"httpGet":{"path":"/nope","port":80}}}]}}}}'
```

Observe:

```bash
kubectl -n mod06 get pods
kubectl -n mod06 describe pod -l app=web | sed -n '1,220p'
kubectl -n mod06 get ep web
```

Expected signals:

- pod may be running but not Ready
- endpoints empty or reduced.

Fix:

```bash
kubectl -n mod06 patch deploy/web -p '{"spec":{"template":{"spec":{"containers":[{"name":"web","readinessProbe":{"httpGet":{"path":"/","port":80}}}]}}}}'
kubectl -n mod06 rollout status deploy/web
kubectl -n mod06 get ep web
```

### 5) Break: OOMKilled (Memory Limit Too Low)

Patch memory limit to an extremely low value and add a side process that consumes memory using a shell loop.

```bash
cat > oom.yaml <<'EOF'
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
spec:
  template:
    spec:
      containers:
        - name: web
          image: nginx:1.27
          command:
            - sh
            - -lc
            - |
              python3 - <<'PY'
              x=[]
              while True:
                x.append("x"*1024*1024)
              PY
              nginx -g "daemon off;"
          resources:
            requests:
              cpu: "50m"
              memory: "32Mi"
            limits:
              cpu: "250m"
              memory: "48Mi"
EOF
kubectl -n mod06 apply -f oom.yaml
```

Observe:

```bash
kubectl -n mod06 get pods
kubectl -n mod06 describe pod -l app=web | sed -n '1,260p'
kubectl -n mod06 get events --sort-by=.lastTimestamp | tail -n 80
```

Expected signals:

- container restarts
- reason shows OOMKilled (often).

Fix: restore a sane memory limit and remove the memory-consuming command by re-applying the original manifest:

```bash
kubectl -n mod06 apply -f k8s.yaml
kubectl -n mod06 rollout status deploy/web
```

## Verify

```bash
kubectl -n mod06 rollout status deploy/web
kubectl -n mod06 get ep web
kubectl -n mod06 port-forward svc/web 18080:80
```

In another terminal:

```bash
curl -fsS http://localhost:18080/ | head
```

Expected signals:

- rollout completes
- endpoints present
- curl returns nginx HTML

## Cleanup

```bash
kubectl delete ns mod06 --wait=true
rm -f k8s.yaml
rm -f oom.yaml
```

Verify cleanup:

```bash
kubectl get ns mod06 || true
```

## Troubleshooting

### Symptom: busybox debug pod fails to start (image pull)

Fix:

- use a different small image already available in your environment
- or pre-pull the image before starting the lab

### Symptom: patch commands fail due to JSON quoting

Fix:

- re-run patches from a file, not inline JSON
- or use `kubectl edit` for the lab and record the final YAML changes

## Why This Matters in Production

- Most Kubernetes incidents are simple but multi-layered: image pulls, labels, probes, and resources.
- Events + describe + endpoints checks often diagnose issues faster than reading app code.

## What to Write in a Runbook

- “Service 503” decision tree: ingress → service → endpoints → readiness → logs
- “Image pull failure” checklist
- “OOMKilled” checklist and safe tuning approach

## Definition of Done

- You can reproduce each failure mode and recover using evidence-based debugging.
