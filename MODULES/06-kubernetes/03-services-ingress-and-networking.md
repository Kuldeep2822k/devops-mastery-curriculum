---
title: Services, Ingress, and Networking (Debug Playbook)
tags:
  - kubernetes
  - networking
  - services
  - ingress
module: "06"
---

# Services, Ingress, and Networking (Debug Playbook)

## Service Types (Operator View)

- ClusterIP: internal virtual IP; backed by endpoints.
- NodePort: opens a port on each node (less common in managed clusters).
- LoadBalancer: cloud integration (cloud extension only).

Critical concept:

- Services route to endpoints selected by labels.

Diagnosis:

```bash
kubectl get svc,endpoints -n <ns>
kubectl describe svc <svc> -n <ns>
kubectl get pods -n <ns> --show-labels
```

## Ingress (High-Level)

Ingress is routing configuration; an ingress controller implements it.

Common outages:

- no ingress controller installed
- wrong host/path rules → misroute or 404
- service has no endpoints → 503

Diagnosis:

```bash
kubectl get ingress -A
kubectl describe ingress <ing> -n <ns>
kubectl get pods -n <ingress-ns>
```

## Debug Playbook: Traffic Failing

1) Is the request reaching the cluster? (ingress/controller logs)  
2) Does ingress route to correct Service? (describe ingress)  
3) Does Service have endpoints? (endpoints not empty)  
4) Are pods Ready? (readiness probe and events)  
5) Is app listening on correct port? (exec + `ss`)  
6) Is DNS working for dependencies? (dns checks)  

## DNS Basics in Kubernetes

Kubernetes DNS failures can cause:

- upstream calls failing
- readiness checks failing

Diagnosis:

```bash
kubectl -n kube-system get pods
kubectl run -n <ns> dns-debug --rm -it --image=busybox:1.36 --restart=Never -- sh -lc "nslookup kubernetes.default.svc.cluster.local || true"
```
