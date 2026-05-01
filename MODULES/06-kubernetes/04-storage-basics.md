---
title: Storage Basics (PVC/PV/StorageClass)
tags:
  - kubernetes
  - storage
module: "06"
---

# Storage Basics (PVC/PV/StorageClass)

## Why Storage Breaks Kubernetes Workloads

Storage introduces:

- ordering constraints (a pod can’t start until storage is bound/attached)
- data durability semantics
- permission and ownership issues

## Core Objects

- PersistentVolumeClaim (PVC): request for storage by a workload
- PersistentVolume (PV): actual provisioned volume
- StorageClass: provisioning policy (dynamic provisioning)

Local clusters (kind/minikube) vary in storage support; labs should treat storage as a “debug the binding” exercise, not as production-grade storage.

## Common Failure Pattern: Pending PVC

Symptoms:

- pod Pending with events mentioning PVC
- PVC stuck in Pending

Diagnosis:

```bash
kubectl get pvc,pv -n <ns>
kubectl describe pvc <pvc> -n <ns>
kubectl get storageclass
kubectl get events -n <ns> --sort-by=.lastTimestamp | tail -n 50
```

Root causes:

- no default StorageClass
- provisioner not running
- size/parameters invalid

## Operator Guidance

- Prefer stateless workloads first; add storage intentionally.
- Always document data paths, backup strategy, and restore steps (later datastore modules deepen this).
