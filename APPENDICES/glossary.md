---
title: 'Glossary'
tags:
  - appendix
  - glossary
---

# Glossary

Plain-English definitions of the terms used throughout this curriculum. **Keep this open in another tab while you read** — especially for Modules 01–05, which introduce a lot of vocabulary quickly. Definitions are intentionally beginner-friendly; you'll get the precise, nuanced version inside the relevant module.

> **Tip:** If a term you need isn't here, that's a bug worth fixing — note it so the glossary can grow.

## Core operations & reliability

- **DevOps** — the practice of using the same people and automation to build, ship, run, and fix software, closing the gap between "development" and "operations."
- **SRE (Site Reliability Engineering)** — an engineering discipline that applies software practices to operations problems (reliability, toil reduction, incident response). Overlaps heavily with DevOps.
- **Operator** — the person (you, here) responsible for running a system safely: deploying changes, watching signals, and restoring service when it breaks.
- **Service** — a running program that responds to requests (e.g. a web API). The thing you operate.
- **Server** — a computer (often remote, often virtual) that runs your services.
- **Uptime / availability** — whether a system is up and reachable *right now*.
- **Reliability** — whether a system meets expectations *over time*, not just once.
- **Durability** — whether stored data survives even when systems fail.
- **SLI (Service Level Indicator)** — a measured signal about a service, e.g. "percentage of requests that succeed."
- **SLO (Service Level Objective)** — a target for an SLI, e.g. "99.9% of requests succeed within 300ms over 7 days."
- **SLA (Service Level Agreement)** — a contractual promise about an SLO, usually with penalties if broken.
- **Error budget** — the amount of unreliability you're allowed before you must slow down and fix things (the flip side of an SLO: a 99.9% SLO permits 0.1% errors).
- **RED metrics** — three core request signals: **R**ate (volume), **E**rrors (failure rate), **D**uration (latency).
- **USE metrics** — for resources: **U**tilization, **S**aturation, **E**rrors.
- **Latency** — how long something takes to respond. Often reported as percentiles: **p50** (median), **p95**, **p99** (the slow tail).
- **Throughput / QPS** — how much work per unit time; QPS = queries (requests) per second.
- **Saturation** — how "full" a resource is (CPU, memory, disk, connection pool). High saturation predicts trouble.

## Incidents & change

- **Incident** — an unplanned disruption to a service that needs a response.
- **On-call** — being the designated responder for incidents during a shift.
- **MTTR (Mean Time To Recovery)** — the average time to restore service after a failure. Lower is better; good runbooks and observability reduce it.
- **Runbook** — a step-by-step operational procedure (diagnose → contain → fix → verify) another engineer can follow under pressure.
- **Postmortem** — a written review after an incident: what happened, why, and how to prevent recurrence (blameless).
- **Blast radius** — how much is affected if a change or failure goes wrong. You try to keep it small.
- **Rollback** — safely returning a system to a previous known-good state after a bad change.
- **Change control** — the discipline of making changes deliberately (small, reversible, observable) rather than ad hoc.
- **ADR (Architecture Decision Record)** — a short document capturing a decision: the context, the options considered, and why one was chosen.
- **Toil** — repetitive manual operational work that could be automated; SRE aims to reduce it.
- **Invariant** — something that must always be true about a system (e.g. "every request carries a trace ID").
- **Failure domain** — a boundary within which things can fail together (a node, a data center, a region).

## Delivery, environments & deployment

- **Environment** — a separated place to run software with different risk levels: **dev** (fast, low assurance), **staging/preprod** (realistic rehearsal), **prod** (real users, strongest controls).
- **CI (Continuous Integration)** — automatically building and testing code every time it changes.
- **CD (Continuous Delivery/Deployment)** — automatically shipping tested code toward (or into) production.
- **Pipeline** — the automated sequence of steps (build → test → deploy) that code passes through.
- **Artifact** — a build output (a container image, a binary, a package) that gets promoted through environments.
- **Promotion** — moving the *same* artifact forward (dev → staging → prod) with increasing scrutiny.
- **Canary** — releasing a change to a small percentage of traffic first to catch problems before a full rollout.
- **Progressive delivery** — rolling out changes gradually (canary, phased, feature-flagged) instead of all at once.
- **Feature flag** — a switch that turns a feature on/off without redeploying, shrinking blast radius.
- **Blue-green deployment** — running two environments and switching traffic between them for instant rollback.
- **Rollout** — the act of deploying a new version across your instances.

## Containers, orchestration & infrastructure

- **Container** — a lightweight, isolated package of an app plus everything it needs to run, so it behaves the same everywhere. (Docker is the common tool.)
- **Image** — the immutable template a container is started from.
- **Registry** — a store for container images (e.g. Docker Hub, GHCR).
- **Kubernetes (k8s)** — a system that runs and manages containers across many machines (scheduling, networking, scaling, self-healing).
- **kind / minikube** — tools that run a local Kubernetes cluster on your laptop for practice.
- **Pod** — the smallest deployable unit in Kubernetes (one or more containers together).
- **Orchestrator** — software that places and manages workloads across machines (Kubernetes is one).
- **IaC (Infrastructure as Code)** — defining infrastructure (servers, networks) in text files so it's versioned and repeatable (e.g. Terraform/OpenTofu).
- **Terraform / OpenTofu** — popular IaC tools; you declare desired state and they make reality match.
- **State (Terraform)** — Terraform's record of what it has created, so it knows what to change.
- **Drift** — when real infrastructure no longer matches what the code says (someone changed it by hand).
- **Idempotent / idempotency** — an operation you can run repeatedly with the same end result (running it twice doesn't double-apply). Central to config management (Ansible).
- **Declarative vs imperative** — *declarative* = "describe the desired end state" (Terraform/Kubernetes); *imperative* = "list the steps to run."
- **Provisioning** — creating and configuring infrastructure/resources.

## Observability

- **Observability** — being able to understand a system's internal state from its outputs.
- **Logs** — timestamped text records of events.
- **Metrics** — numeric measurements over time (CPU %, request rate).
- **Traces** — records that follow a single request across multiple services.
- **Dashboard** — a visual view of metrics/logs, ideally tied to user-facing behavior.
- **Alert** — an automated notification when a signal crosses a threshold; good alerts link to a runbook.
- **Health check / `/healthz`** — an endpoint that reports whether a service is OK, used by load balancers and orchestrators.
- **Structured logs** — logs emitted as consistent key/value (often JSON) so machines can parse them.

## Networking, security & identity

- **Proxy / reverse proxy** — a server that sits in front of your services and forwards requests (handling routing, TLS, buffering).
- **Load balancer** — distributes incoming requests across multiple instances.
- **DNS** — the system that turns names (`example.com`) into IP addresses.
- **Split-horizon DNS** — returning different DNS answers depending on where the request comes from (internal vs external).
- **TLS / SSL** — encryption for network traffic (the "S" in HTTPS); depends on correct clocks and certificates.
- **Certificate (cert) / CA** — a cryptographic document proving identity; a Certificate Authority (CA) issues and vouches for them.
- **mTLS (mutual TLS)** — both client and server prove identity to each other with certificates.
- **Secret** — sensitive data (password, API key, token) that must never be logged or committed.
- **Least privilege** — granting only the minimum access needed, and designing so access can be revoked.
- **RBAC (Role-Based Access Control)** — granting permissions via roles rather than to individuals directly.
- **OAuth / OIDC** — standards for authorization (OAuth) and authentication/login (OIDC) using tokens.
- **JWT** — a signed token carrying claims (who you are, what you can do) that services can verify.
- **Workload identity** — giving a *service* (not a human) a short-lived, verifiable identity instead of long-lived secrets.
- **Supply chain (software)** — everything that goes into producing your artifact (dependencies, build steps); a target for attacks.
- **SBOM (Software Bill of Materials)** — a list of everything inside an artifact, for auditing and vulnerability tracking.
- **Provenance** — evidence about how an artifact was produced (who/what/when), used to trust it.
- **Signing / attestation** — cryptographically vouching for an artifact's origin and integrity.

## Data & messaging

- **Datastore / database** — where persistent data lives (e.g. Postgres).
- **Cache** — fast temporary storage for frequently used data (e.g. Redis).
- **Backup / restore** — copying data so it can be recovered; a backup you've never *restored* is a guess.
- **DR (Disaster Recovery)** — the plan and capability to recover after a major failure.
- **Message queue** — a buffer that holds messages between producers and consumers (decouples systems).
- **DLQ (Dead Letter Queue)** — where messages go when they repeatedly fail processing, so they don't block everything else.
- **Consumer lag** — how far behind a consumer is from the latest messages; a key health signal for queues/streams.
- **Consistency vs availability** — a fundamental tradeoff in distributed systems: prioritize always-correct data, or always-answering service, when the network splits.

## Cost & platform

- **FinOps** — managing cloud cost as an engineering concern (budgets, tagging, guardrails).
- **Platform engineering** — building internal "golden paths" and self-service tooling so product teams ship safely without reinventing ops.
- **Golden path** — a supported, paved way to do a common task (deploy a service) that has best practices built in.
- **Guardrails** — automated limits that keep people within safe boundaries (vs. gates that block them).
