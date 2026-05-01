---
title: Checklist (Definition of Done)
tags:
  - checklist
  - kubernetes
module: "06"
---

# Checklist — Module 06 Kubernetes (DoD)

- [ ] Explain core primitives (Pod, Deployment, Service, Ingress) and their operational responsibilities.
- [ ] Demonstrate rollout monitoring:
  - [ ] `kubectl rollout status`
  - [ ] interpret events during rollout
- [ ] Configure probes correctly and explain readiness vs liveness vs startup.
- [ ] Configure resource requests/limits and explain failure modes (Pending, OOMKilled, throttling).
- [ ] Complete Lab 01 (deploy + break + fix) with evidence:
  - [ ] ImagePullBackOff
  - [ ] label mismatch → endpoints empty
  - [ ] probe failure → not Ready
  - [ ] OOMKilled (or equivalent evidence of memory failure)
- [ ] Complete Lab 02 (network + DNS debug) with evidence:
  - [ ] DNS name mismatch diagnosis
  - [ ] Service selector mismatch diagnosis
- [ ] Complete at least 12 scenarios from [troubleshooting-lab.md](troubleshooting-lab.md) with written diagnosis and verification.
- [ ] Complete [exam.md](exam.md) with commands, expected signals, and reasoning.
- [ ] Write a runbook using [runbook-template.md](runbook-template.md) for “Service 503 / pods crash looping”.
- [ ] Write one ADR using [decision-record-template.md](decision-record-template.md) about probes/resources or workload pattern tradeoff.
- [ ] Self-grade via [rubric.md](rubric.md) and list 3 improvement actions.
