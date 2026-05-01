---
title: 'RBAC Failure Drill'
tags:
  - lab
  - rbac
  - debugging
module: "22"
---

# Lab 02 — RBAC Failure Drill (Kubernetes can-i + Least Privilege)

## Goal

Practice debugging RBAC “forbidden” failures safely:

- reproduce a forbidden error for a service account
- isolate the missing verb/resource via `kubectl auth can-i`
- fix with least privilege (not cluster-admin)
- verify permissions change and behavior recovers

## Prereqs

- local Kubernetes cluster running (kind/minikube)
- `kubectl` configured to talk to it

## Setup

```bash
kubectl create ns mod22-rbac || true
kubectl -n mod22-rbac create sa reader || true
```

Create an intentionally under-privileged Role (missing `list`):

```bash
cat > rbac.yaml <<'EOF'
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: pod-reader
  namespace: mod22-rbac
rules:
  - apiGroups: [""]
    resources: ["pods"]
    verbs: ["get"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: pod-reader
  namespace: mod22-rbac
subjects:
  - kind: ServiceAccount
    name: reader
    namespace: mod22-rbac
roleRef:
  kind: Role
  name: pod-reader
  apiGroup: rbac.authorization.k8s.io
EOF
kubectl apply -f rbac.yaml
```

Create a simple pod to list:

```bash
kubectl -n mod22-rbac run hello --image=busybox:1.36 --restart=Never -- sh -lc "sleep 3600" || true
kubectl -n mod22-rbac get pod hello -o wide
```

## Steps

### 1) Reproduce Forbidden

Impersonate the service account and attempt to list pods:

```bash
kubectl -n mod22-rbac auth can-i list pods --as=system:serviceaccount:mod22-rbac:reader || true
kubectl -n mod22-rbac get pods --as=system:serviceaccount:mod22-rbac:reader || true
```

Expected signals:

- `can-i` returns `no`
- `get pods` is forbidden

### 2) Narrow the Missing Permission

```bash
kubectl -n mod22-rbac auth can-i get pods --as=system:serviceaccount:mod22-rbac:reader
kubectl -n mod22-rbac auth can-i list pods --as=system:serviceaccount:mod22-rbac:reader || true
kubectl -n mod22-rbac describe role pod-reader | sed -n '1,120p'
```

Expected:

- `get` is allowed
- `list` is denied

### 3) Fix With Least Privilege

Patch Role to include `list`:

```bash
kubectl -n mod22-rbac patch role pod-reader --type='json' \
  -p='[{"op":"add","path":"/rules/0/verbs/-","value":"list"}]'
```

### 4) Verify Recovery

```bash
kubectl -n mod22-rbac auth can-i list pods --as=system:serviceaccount:mod22-rbac:reader
kubectl -n mod22-rbac get pods --as=system:serviceaccount:mod22-rbac:reader
```

Expected:

- `can-i` returns `yes`
- `get pods` succeeds

## Verify

```bash
kubectl -n mod22-rbac auth can-i list pods --as=system:serviceaccount:mod22-rbac:reader
kubectl -n mod22-rbac get pods --as=system:serviceaccount:mod22-rbac:reader | grep -q '^hello' && echo "ok: rbac fixed"
```

Expected signals:

- final line prints `ok: rbac fixed`

## Cleanup

```bash
kubectl delete ns mod22-rbac --wait=true
rm -f rbac.yaml
```

## Troubleshooting

### Symptom: can-i says yes but request still forbidden

Fix:

- confirm you are impersonating the correct service account
- re-check RoleBinding subject namespace/name
