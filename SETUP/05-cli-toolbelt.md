---
title: CLI Toolbelt
tags:
  - setup
  - cli
  - troubleshooting
---

# CLI Toolbelt

## Goal

Build a CLI toolbelt that supports fast diagnosis across layers: host, network, TLS, containers, Kubernetes, and CI.

## Philosophy

Tools are only valuable if you can use them under pressure. Prefer a small set you practice repeatedly over a huge set you never touch.

## Core Tools (Required)

### Shell and Build

- `bash` or `zsh`
- `make`
- `python3` (for quick scripts)

Verification:

```bash
make --version
python3 --version
```

### Data and APIs

- `curl`
- `jq`

Verification:

```bash
curl --version
jq --version
```

### Git

- `git`

Verification:

```bash
git --version
```

### Kubernetes

- `kubectl`

Verification:

```bash
kubectl version --client
kubectl config current-context
```

## Networking and TLS Tools (Strongly Recommended)

- DNS: `dig` or `nslookup`
- sockets: `ss` (Linux) or `netstat`
- connection tests: `nc`
- TLS inspection: `openssl`
- packet capture (advanced): `tcpdump`

Verification:

```bash
dig example.com +short || nslookup example.com
ss -tulpn || netstat -tulpn
openssl version
```

## Text, Search, and Log Handling

- `grep`, `sed`, `awk`
- `less`
- `ripgrep` (`rg`) if available

Operator patterns:

- search for “first failure” not “final summary”
- capture a small, relevant window of logs around the event

## Container Debug Tools

- `docker`

Verification:

```bash
docker ps
```

## Kubernetes Debug Muscle Memory (Starter Set)

These commands should become reflexive:

```bash
kubectl get pods -A
kubectl get events -A --sort-by=.lastTimestamp
kubectl describe pod <pod>
kubectl logs <pod> --previous
kubectl exec -it <pod> -- sh
kubectl get svc,endpoints -A
```

## Troubleshooting

### Symptom: kubectl works sometimes, not always

Diagnosis:

- check context and namespace: `kubectl config get-contexts`
- check kubeconfig path and permissions

Fix:

- explicitly set context
- reduce kubeconfig clutter by using a single config per lab machine if possible

### Symptom: curl works on host, fails in cluster

Diagnosis:

- host DNS vs cluster DNS
- proxy settings (host may have proxy; pods may not)

Fix:

- align proxy/DNS configuration or explicitly bypass where appropriate
- treat this as a real incident scenario; do not “just disable TLS”

## Why This Matters in Production

- Tooling gaps create MTTR.
- The same command sets appear in almost every incident; practicing them is compounding returns.
