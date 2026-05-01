---
title: Checklist (Definition of Done)
tags:
  - checklist
  - security
module: "10"
---

# Checklist — Module 10 Security (DoD)

- [ ] Write a simple threat model for a small service (assets, entry points, trust boundaries).
- [ ] Demonstrate secrets hygiene:
  - [ ] no secrets in repo
  - [ ] no secrets printed to logs
  - [ ] rotation plan if leaked
- [ ] Implement a baseline guardrail:
  - [ ] secret scan gate (local or CI)
  - [ ] base image tag pinning rule (no :latest)
- [ ] Complete Lab 01 (git hook blocks secret-like commits).
- [ ] Complete Lab 02 (policy gate script with at least 2 checks).
- [ ] Complete at least 12 scenarios from [troubleshooting-lab.md](troubleshooting-lab.md).
- [ ] Complete [exam.md](exam.md) with evidence (commands + expected signals + reasoning).
- [ ] Write a security runbook using [runbook-template.md](runbook-template.md) for “secret leaked” or “suspicious CI behavior”.
- [ ] Write one ADR using [decision-record-template.md](decision-record-template.md) about security gates and least privilege.
- [ ] Self-grade via [rubric.md](rubric.md) and list 3 improvement actions.
