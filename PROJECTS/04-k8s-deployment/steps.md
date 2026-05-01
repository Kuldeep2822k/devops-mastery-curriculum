---
title: '04-k8s-deployment: Steps'
tags:
  - project
---

# 04-k8s-deployment — Steps

## Prereqs

- local Kubernetes cluster (kind or minikube)
- `kubectl`
- `curl`

## Setup

```bash
kubectl create ns proj04 || true
```

Create a minimal deployment + service:

```bash
cat > k8s.yaml <<'EOF'
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
  namespace: proj04
spec:
  replicas: 2
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
---
apiVersion: v1
kind: Service
metadata:
  name: web
  namespace: proj04
spec:
  selector:
    app: web
  ports:
    - port: 80
      targetPort: 80
EOF
kubectl apply -f k8s.yaml
kubectl -n proj04 rollout status deploy/web
```

## Steps

### 1) Verify Service Routing

```bash
kubectl -n proj04 get pods -o wide
kubectl -n proj04 get svc,ep
kubectl -n proj04 port-forward svc/web 18081:80
```

In another terminal:

```bash
curl -fsS http://127.0.0.1:18081/ | head
```

### 2) Break/Fix: Service Selector Mismatch (No Endpoints)

Break:

```bash
kubectl -n proj04 patch svc web -p '{"spec":{"selector":{"app":"wrong"}}}'
kubectl -n proj04 get ep web
curl -fsS http://127.0.0.1:18081/ || true
```

Fix:

```bash
kubectl -n proj04 patch svc web -p '{"spec":{"selector":{"app":"web"}}}'
kubectl -n proj04 get ep web
```

### 3) Break/Fix: Readiness Failure

Break (wrong probe path):

```bash
kubectl -n proj04 patch deploy web --type='json' -p='[{"op":"replace","path":"/spec/template/spec/containers/0/readinessProbe/httpGet/path","value":"/does-not-exist"}]'
kubectl -n proj04 rollout status deploy/web || true
kubectl -n proj04 get pods
kubectl -n proj04 describe pod -l app=web | sed -n '1,120p'
```

Fix:

```bash
kubectl -n proj04 patch deploy web --type='json' -p='[{"op":"replace","path":"/spec/template/spec/containers/0/readinessProbe/httpGet/path","value":"/"}]'
kubectl -n proj04 rollout status deploy/web
```

### 4) Rollback Drill

Deploy a bad image:

```bash
kubectl -n proj04 set image deploy/web web=nginx:does-not-exist || true
kubectl -n proj04 rollout status deploy/web || true
kubectl -n proj04 rollout history deploy/web
```

Rollback:

```bash
kubectl -n proj04 rollout undo deploy/web
kubectl -n proj04 rollout status deploy/web
```

## Verify

```bash
kubectl -n proj04 get ep web | grep -q ':' && echo ok
kubectl -n proj04 rollout status deploy/web
```

## Cleanup

```bash
kubectl delete ns proj04 --wait=true
rm -f k8s.yaml
```

## Troubleshooting

- Use: `kubectl -n proj04 get events --sort-by=.lastTimestamp | tail -n 30`
- For routing: endpoints must exist and pods must be Ready.
