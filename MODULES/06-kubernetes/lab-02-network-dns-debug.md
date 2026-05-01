---
title: "Lab 02: Network + DNS Debug Lab"
tags:
  - lab
  - kubernetes
  - networking
  - dns
module: "06"
---

# Lab 02 — Network + DNS Debug Lab

## Goal

Practice a network/DNS debugging workflow inside Kubernetes:

- create a client and server in a namespace
- verify Service DNS resolution
- break DNS usage (wrong name) and diagnose
- break Service selectors and diagnose
- validate “from inside pod” vs “from host” differences

## Prereqs

- local cluster running
- `kubectl`
- ability to pull a small debug image (busybox)

## Setup

Create namespace and server:

```bash
kubectl create ns mod06-net || true

cat > net.yaml <<'EOF'
apiVersion: apps/v1
kind: Deployment
metadata:
  name: echo
  labels:
    app: echo
spec:
  replicas: 1
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
          args: ["-text=ok"]
          ports:
            - containerPort: 5678
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

kubectl -n mod06-net apply -f net.yaml
kubectl -n mod06-net rollout status deploy/echo
kubectl -n mod06-net get svc,ep,pods -o wide
```

## Steps

### 1) Verify Service Works via DNS From Inside Cluster

Run a debug pod and curl the service name:

```bash
kubectl -n mod06-net run client --rm -it --restart=Never --image=busybox:1.36 -- sh -lc "wget -qO- http://echo || true"
```

Expected signal:

- output `ok`

Verify DNS resolution:

```bash
kubectl -n mod06-net run dns --rm -it --restart=Never --image=busybox:1.36 -- sh -lc "nslookup echo || true; nslookup echo.mod06-net.svc.cluster.local || true"
```

### 2) Break: Wrong DNS Name

Simulate an app configured with the wrong host:

```bash
kubectl -n mod06-net run client --rm -it --restart=Never --image=busybox:1.36 -- sh -lc "wget -qO- http://echoo || true"
```

Diagnosis commands:

- `nslookup echoo`
- verify correct service name: `kubectl -n mod06-net get svc`

Root cause:

- wrong hostname; service name mismatch.

Fix:

- use the correct name `echo` or FQDN.

### 3) Break: Service Selector Mismatch (No Endpoints)

Patch service selector:

```bash
kubectl -n mod06-net patch svc/echo -p '{"spec":{"selector":{"app":"echo-broken"}}}'
kubectl -n mod06-net get ep echo
```

Expected:

- endpoints empty.

From client:

```bash
kubectl -n mod06-net run client --rm -it --restart=Never --image=busybox:1.36 -- sh -lc "wget -S -qO- http://echo || true"
```

Diagnosis:

```bash
kubectl -n mod06-net describe svc echo
kubectl -n mod06-net get pods --show-labels
kubectl -n mod06-net get ep echo -o yaml
```

Fix:

```bash
kubectl -n mod06-net patch svc/echo -p '{"spec":{"selector":{"app":"echo"}}}'
kubectl -n mod06-net get ep echo
```

### 4) Compare “Host vs Pod” DNS

From host, `echo` is not a DNS name:

```bash
nslookup echo 2>/dev/null || true
```

Expected:

- host cannot resolve cluster service name.

This teaches:

- service DNS is internal to cluster DNS.

## Verify

```bash
kubectl -n mod06-net get ep echo
kubectl -n mod06-net run client --rm -it --restart=Never --image=busybox:1.36 -- sh -lc "wget -qO- http://echo || true"
```

Expected:

- endpoints present
- output `ok`

## Cleanup

```bash
kubectl delete ns mod06-net --wait=true
rm -f net.yaml
```

## Troubleshooting

### Symptom: busybox image pull fails

Fix:

- use an alternative small debug image available in your environment
- or pre-pull the image on the node

### Symptom: cluster has no DNS pods

Diagnosis:

```bash
kubectl -n kube-system get pods
```

Fix:

- recreate the cluster or fix the DNS addon depending on your local distro

## Why This Matters in Production

- Many incidents are “DNS is broken” but are actually config mistakes or endpoint/selector issues.
- Debugging from inside a pod is often the only way to reproduce network and DNS behavior accurately.

## What to Write in a Runbook

- how to test Service DNS from inside cluster
- how to check endpoints and selector mismatches
- how to distinguish DNS failure from connectivity failure

## Definition of Done

- You can break and fix DNS usage and Service endpoint routing with clear evidence.
