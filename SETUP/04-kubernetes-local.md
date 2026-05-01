---
title: Kubernetes Local (kind/minikube)
tags:
  - setup
  - kubernetes
  - local-first
---

# Kubernetes Local (kind/minikube)

## Goal

Create a local Kubernetes cluster suitable for deep debugging and failure injection (pods, services, ingress, DNS, scheduling, resource pressure).

## Prereqs

- Docker working (verify with `docker run --rm hello-world`)
- kubectl installed (covered in [05-cli-toolbelt.md](05-cli-toolbelt.md))

## Choose a Local Cluster Option

- kind (recommended): runs Kubernetes nodes as containers; fast and predictable
- minikube: flexible drivers; may be heavier but has built-in addons

Use one as your “default” so your muscle memory stays consistent.

## Setup (kind)

### Install kind

Install via your package manager or official binaries.

Verify:

```bash
kind version
```

### Create a Cluster

Use a named cluster to avoid collisions:

```bash
kind create cluster --name devops-lab
```

Verify:

```bash
kind get clusters
kubectl cluster-info
kubectl get nodes
```

Expected signals:

- cluster appears in `kind get clusters`
- `kubectl get nodes` shows Ready nodes

### Create a Default Namespace for Labs

```bash
kubectl create ns labs
kubectl config set-context --current --namespace=labs
kubectl get ns
```

Expected signals:

- `labs` namespace exists
- current context namespace is `labs`

## Setup (minikube)

### Start a Cluster

```bash
minikube start
```

Verify:

```bash
minikube status
kubectl get nodes
```

Expected signals:

- minikube reports running
- node is Ready

## Core Addons You’ll Use Later

You can choose to enable:

- metrics-server (for resource visibility)
- ingress controller (for ingress labs)

Enable only when needed; more addons increase complexity.

## Cluster Hygiene

### Context Management

Know what cluster you are pointing at:

```bash
kubectl config get-contexts
kubectl config current-context
```

### Reset to Clean State

Preferred pattern:

- delete namespaces created by labs
- delete the cluster when you need a full reset

kind cleanup:

```bash
kind delete cluster --name devops-lab
kind get clusters
```

Expected signals:

- cluster removed

## Troubleshooting

### Symptom: kubectl cannot connect

Diagnosis:

- `kubectl config current-context`
- `kubectl cluster-info`

Fix:

- select correct context
- recreate cluster if kubeconfig is stale

### Symptom: nodes NotReady

Diagnosis:

- `kubectl describe node <node>`
- `kubectl get events -A --sort-by=.lastTimestamp`

Fix:

- check Docker resources (CPU/mem/disk)
- recreate cluster after fixing host resource pressure

### Symptom: DNS issues inside cluster

Diagnosis:

- `kubectl -n kube-system get pods`
- run a debug pod and test `nslookup`/`dig`

Fix:

- restart cluster
- ensure your host DNS/proxy configuration is not breaking container networking

## Why This Matters in Production

- Kubernetes failures are usually multi-layer: app + YAML + cluster + network + node pressure.
- A local cluster lets you practice failure injection safely and learn real debug playbooks before touching cloud clusters.
