---
title: Roadmap
tags:
  - roadmap
  - curriculum
---

# Roadmap

This roadmap explains the curriculum flow and how to turn modules into staff-level capability (not “tool familiarity”).

## Learning Contract

- You do labs with verification signals, not just “it worked once”.
- You write runbooks and ADRs for everything you build.
- You practice failure injection and time-boxed incident response.
- You use rubrics to self-grade and to target weaknesses.

Operating model: [00-how-to-use.md](00-HOW-TO-USE/00-how-to-use.md)

## Module Progression (01–25)

### Core (01–12): Operational Fundamentals and SRE Core

- 01 Foundations: systems thinking, environments, change control, reliability basics
- 02 Linux: process/service management, storage, perf, debugging
- 03 Git: collaboration, history shaping, workflows, repo hygiene
- 04 CI: pipeline architecture, artifacts, caching, secrets, reliability of CI itself
- 05 Containers: images, registries, runtime debugging, isolation, resource controls
- 06 Kubernetes: workload patterns, networking, storage, scheduling, deep debugging playbooks
- 07 Delivery: CD, deployment strategies, rollbacks, progressive delivery patterns
- 08 IaC (Terraform): state/locking/drift, module design, lifecycle control, safe changes
- 09 Config Mgmt (Ansible): fleets, idempotency, secrets hygiene, inventory design, troubleshooting
- 10 Security: threat modeling for operators, supply chain, policies, secrets, hardening
- 11 Observability: metrics/logs/traces, alerting design, SLOs, dashboards as products
- 12 SRE: incident management, error budgets, toil, capacity, reliability tradeoffs

### Expansion (13–19): Staff Breadth in Real Systems

- 13 Programming for DevOps: scripting, automation, API usage, reliability in code
- 14 Web Proxy: reverse proxies, load balancers, timeouts, buffering, headers, auth edges
- 15 Networking Protocols: TCP/UDP, DNS, HTTP, TLS, routing, failure patterns
- 16 Serverless: eventing, cold starts, concurrency, operational model differences
- 17 Artifacts: artifact stores, provenance, promotion by digest, SBOM, signing, attestations
- 18 Service Mesh: when to use, when not, mTLS, traffic policy, failure domains
- 19 Email: SMTP basics, deliverability, operational debugging (often hits production)

### Staff-Level Depth (20–25): Data, Identity, Release, Platforms, Cost

- 20 Datastores for DevOps: Postgres/Redis operational baseline, failure modes, backups, performance
- 21 Message Queues & Streaming: retries/DLQ/idempotency/ordering/lag; operational triage
- 22 Identity and Access: OIDC/OAuth, RBAC, workload identity, least privilege debugging
- 23 Release Engineering: versioning, changelogs, gates, signing/SBOM, rollback drills, progressive delivery
- 24 Platform Engineering: golden paths, templates, guardrails, multi-tenancy, IDP concepts
- 25 Cost/Reliability/FinOps: cost drivers, budgets, tagging, guardrails, reliability vs cost decisions

## How to “Finish” a Module

Each module includes:

- concept lessons (01–04): short, applied
- deep dives (deep-dive-01/02): tradeoffs, edge cases, anti-patterns
- at least two labs (lab-01/02) with verification + cleanup + troubleshooting
- cloud-extension-lab.md (optional) with cost control
- checklist.md (Definition of Done) and rubric.md (0–4 scoring per skill)
- exam.md (practical) and troubleshooting-lab.md (12–20 scenarios)
- runbook + ADR templates tuned to the module

Evidence and grading: [04-evidence-rubrics.md](00-HOW-TO-USE/04-evidence-rubrics.md)

## Projects and Capstones

Modules build skills; projects test integration; capstones force staff-level design tradeoffs and documentation.

- Projects: PROJECTS/
- Capstones: CAPSTONES/
- Brownfield Rescue: intentionally broken systems; stabilize CI, fix infra, reduce MTTR, cut alert noise, create SLOs, write postmortems

## Schedules (Pick One)

- SCHEDULES/30-day-boot.md (survival path)
- SCHEDULES/90-day-deep.md
- SCHEDULES/180-day-staff.md
- SCHEDULES/365-day-mastery.md

## Cross-Links Index

- How to use: [00-HOW-TO-USE](00-HOW-TO-USE/00-how-to-use.md)
- Setup: SETUP/00-overview.md
- Troubleshooting catalog: TROUBLESHOOTING_CATALOG/00-index.md
- Cheatsheets: CHEATSHEETS/cheatsheets-index.md
- Interview drills: INTERVIEW_DRILLS/00-overview.md
- Progress tracking: PROGRESS/learning-checklist.md
- Appendices: APPENDICES/00-index.md
