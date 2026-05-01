---
title: Reviewer Module Map
tags:
  - reviewer
  - map
---

# Module-by-Module Map (01–25)

## 01-foundations

Learning outcomes:
- Describe a service as an operational system (dependencies, failure modes, invariants, and signals).
- Define environments (dev/stage/prod) with separation goals and a minimal promotion model.
- Build a minimal “operationally ready” service locally: health checks, logs, basic SLO thinking.
- Execute a safe change with rollback and capture an incident log under time pressure.
- Write a basic runbook and a decision record (ADR) that captures tradeoffs.

Files included:
- 00-overview.md
- 01-systems-thinking.md
- 02-environments-and-change.md
- 03-reliability-basics.md
- 04-operational-writing.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-local-service-ops-baseline.md
- lab-02-change-and-rollback-drill.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-local-service-ops-baseline.md: local-first drill with verification + cleanup evidence
- lab-02-change-and-rollback-drill.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain the service-as-a-system model for a simple service (dependencies, failure modes, signals).
- Define at least 3 invariants for the lab service (e.g., health endpoint behavior, latency target, logging format).
- Create a minimal operational interface:
- Complete Lab 01 with:
- Complete Lab 02 with:

Most important troubleshooting scenarios (top 5):
- Port Already In Use
- Service Running But Health Fails
- Wrong Port Assumption
- Latency Regression Without Errors
- Flaky Health (Intermittent Failures)

Practical exam tasks (summary):
- Build a Minimal Service with Operational Interfaces
- Failure Injection and Recovery
- Write a Mini Runbook and an ADR

Estimated effort level: Medium — Foundational skills with intensive repetition and troubleshooting practice.

## 02-linux

Learning outcomes:
- Triage a Linux system quickly (CPU, memory, disk, IO, network, processes).
- Understand and debug process lifecycle, signals, and common failure patterns.
- Use systemd/journald for service operations (start/stop/status/logs, unit inspection).
- Diagnose common “service down” incidents: port binding, permission, config, dependencies, resource pressure.
- Apply safe operational changes: limits, file descriptors, sysctls (scoped and reversible), and document them.

Files included:
- 00-overview.md
- 01-processes-and-signals.md
- 02-filesystems-and-permissions.md
- 03-systemd-and-logging.md
- 04-network-triage-linux.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-systemd-service-debug.md
- lab-02-resource-pressure-drill.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-systemd-service-debug.md: local-first drill with verification + cleanup evidence
- lab-02-resource-pressure-drill.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Perform a 5-minute Linux triage and explain what each command reveals (CPU/mem/disk/network).
- Explain process lifecycle, signals, and safe termination (TERM vs KILL).
- Demonstrate file permission reasoning:
- Demonstrate systemd/journald competence:
- Complete Lab 01:

Most important troubleshooting scenarios (top 5):
- Service Not Running
- Bad ExecStart Path
- Wrong Working Directory
- Permission Denied Reading Config
- Port Already In Use

Practical exam tasks (summary):
- Triage a “Service Down” Host
- systemd Unit Debug
- Resource Pressure Diagnosis
- Write Runbook + ADR

Estimated effort level: Medium — Foundational skills with intensive repetition and troubleshooting practice.

## 03-git

Learning outcomes:
- Use Git as an operational tool: trace changes, identify regressions, perform safe rollbacks.
- Work confidently with branching strategies, merges, and conflict resolution.
- Use history tools under pressure: reflog, bisect, blame (with nuance), and targeted reverts.
- Design repository hygiene: commit structure, release tags, and change logs that support incident forensics.
- Apply secure and safe collaboration patterns: branch protection intent, code review discipline, and least surprise history practices.

Files included:
- 00-overview.md
- 01-git-mental-model.md
- 02-branching-and-merge-strategies.md
- 03-history-tools-for-operators.md
- 04-collaboration-and-review.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-branching-conflicts-and-recovery.md
- lab-02-regression-hunt-bisect-and-revert.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-branching-conflicts-and-recovery.md: local-first drill with verification + cleanup evidence
- lab-02-regression-hunt-bisect-and-revert.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain Git objects (commit/tree/blob) and refs (branches, tags, HEAD) using a concrete example.
- Demonstrate safe vs risky operations in shared repos (revert vs reset/rebase) and explain why.
- Resolve at least one merge conflict correctly and verify result via file content and history.
- Recover from a destructive local operation using reflog and explain what was recovered.
- Use bisect with a deterministic signal to identify a first-bad commit.

Most important troubleshooting scenarios (top 5):
- “Detached HEAD” After Checking a Commit
- Local Hard Reset Lost a Commit
- Rebase Gone Wrong
- Push Rejected (Non-Fast-Forward)
- Accidentally Committed to main

Practical exam tasks (summary):
- Conflict and Merge Under Constraints
- Recovery Drill (Reflog)
- Regression Hunt (Bisect)
- Safe Rollback (Revert) + Tag
- Runbook + ADR

Estimated effort level: Medium — Foundational skills with intensive repetition and troubleshooting practice.

## 04-ci

Learning outcomes:
- Design CI pipeline architecture: stages, artifacts, caching, parallelism, fan-in/fan-out.
- Build a portable local-first pipeline using Makefile targets and a local runner simulation.
- Build a GitHub Actions pipeline with caching, artifacts, fail-fast, and smoke tests.
- Handle secrets safely: environment separation, approvals, and least-privilege access.
- Improve CI reliability: flaky tests, nondeterminism, timeouts, reruns, quarantines, and pipeline health metrics.
- Troubleshoot CI failures using evidence: logs, artifact inspection, environment diffs, lockfile integrity, and provenance basics.

Files included:
- 00-overview.md
- 01-ci-pipeline-architecture.md
- 02-artifacts-caching-parallelism.md
- 03-secrets-environments-approvals.md
- 04-ci-reliability.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-portable-pipeline-local-runner.md
- lab-02-github-actions-pipeline.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-portable-pipeline-local-runner.md: local-first drill with verification + cleanup evidence
- lab-02-github-actions-pipeline.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain a CI pipeline architecture with stages, artifacts, caching, and gates.
- Define artifact contracts for your pipeline (what is produced/consumed and where).
- Implement a portable pipeline using Make targets and verify:
- Implement a GitHub Actions pipeline and verify:
- Explain and apply safe secrets handling:

Most important troubleshooting scenarios (top 5):
- Workflow Trigger Not Firing
- Lockfile Drift Breaks Build
- Missing dist/ Directory
- Secrets Missing in Environment
- Slow Pipelines (Cache Ineffective)

Practical exam tasks (summary):
- Portable Pipeline Contract
- Clean Runner Reproduction
- GitHub Actions Implementation
- CI Reliability Drill
- Runbook + ADR

Estimated effort level: Medium — Requires two labs plus troubleshooting drills, rubric scoring, and a practical exam.

## 05-containers

Learning outcomes:
- Build container images reproducibly (no “works on my machine” drift).
- Understand image layers, tags vs digests, and why promotion by digest matters.
- Operate containers: logs, exec, inspect, networking, volumes, resource limits.
- Debug common container runtime failures quickly: image pull issues, permissions, missing files, env/config errors, OOM, port binding, DNS.
- Apply secure defaults: least privilege, no secrets baked into images, minimized base images, SBOM mindset.

Files included:
- 00-overview.md
- 01-container-mental-model.md
- 02-images-layers-tags-digests.md
- 03-runtime-ops-debugging.md
- 04-secure-container-practices.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-build-run-debug-image.md
- lab-02-break-fix-runtime-failures.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-build-run-debug-image.md: local-first drill with verification + cleanup evidence
- lab-02-break-fix-runtime-failures.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain image vs container and why container changes are not a deploy strategy.
- Explain tags vs digests and why promotion by digest matters for rollback and provenance.
- Build an image and run a container with port mapping and health verification.
- Demonstrate core ops commands:
- Complete Lab 01 with Verify + Cleanup evidence.

Most important troubleshooting scenarios (top 5):
- Container Exits Immediately (Wrong Command)
- Port Mapping Wrong
- App Bound to 127.0.0.1 Inside Container
- Permission Denied Writing to Volume
- DNS Failure Inside Container

Practical exam tasks (summary):
- Build and Run
- Evidence-Based Debug
- Resource Limits Drill
- Runbook + ADR

Estimated effort level: Medium — Requires two labs plus troubleshooting drills, rubric scoring, and a practical exam.

## 06-kubernetes

Learning outcomes:
- Explain core Kubernetes primitives and how they compose into production systems.
- Deploy workloads with correct labels/selectors, health probes, and resource requests/limits.
- Debug outages using an evidence-based playbook: events → describe → logs → exec → network/DNS checks.
- Diagnose scheduling issues (Pending), pull failures, crash loops, probe failures, OOM, and traffic routing failures (Service/Ingress).
- Apply basic RBAC troubleshooting and least privilege thinking.
- Write Kubernetes runbooks and ADRs for workload patterns and operational tradeoffs.

Files included:
- 00-overview.md
- 01-workloads-labels-and-rollouts.md
- 02-probes-resources-and-scheduling.md
- 03-services-ingress-and-networking.md
- 04-storage-basics.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-deploy-break-fix.md
- lab-02-network-dns-debug.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-deploy-break-fix.md: local-first drill with verification + cleanup evidence
- lab-02-network-dns-debug.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain core primitives (Pod, Deployment, Service, Ingress) and their operational responsibilities.
- Demonstrate rollout monitoring:
- Configure probes correctly and explain readiness vs liveness vs startup.
- Configure resource requests/limits and explain failure modes (Pending, OOMKilled, throttling).
- Complete Lab 01 (deploy + break + fix) with evidence:

Most important troubleshooting scenarios (top 5):
- CrashLoopBackOff (Bad Config)
- ImagePullBackOff
- Pending (Insufficient Resources)
- Pending (Node Selector / Taints)
- OOMKilled

Practical exam tasks (summary):
- Deploy a Service Correctly
- Debug Drill (Pick 3 Failures)
- Runbook + ADR

Estimated effort level: High — Involves multiple subsystems, failure modes, and operational writing deliverables.

## 07-delivery

Learning outcomes:
- Design a delivery pipeline that promotes the same artifact across environments.
- Choose deployment strategies (rolling, blue/green, canary) based on risk and constraints.
- Execute rollouts and rollbacks safely using measurable signals and clear triggers.
- Handle config and schema change coordination (migrations) with rollback safety in mind.
- Debug delivery incidents: wrong version, stuck rollout, unhealthy canary, misrouted traffic, config drift.
- Write runbooks and ADRs that capture delivery tradeoffs and operational responsibilities.

Files included:
- 00-overview.md
- 01-cd-and-promotion-model.md
- 02-deployment-strategies.md
- 03-rollback-and-migration-safety.md
- 04-release-gates-and-progressive-delivery.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-rollout-and-rollback-kind.md
- lab-02-canary-and-promotion.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-rollout-and-rollback-kind.md: local-first drill with verification + cleanup evidence
- lab-02-canary-and-promotion.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain “build once, promote many” and why rebuild-per-env breaks provenance.
- Compare rolling, canary, and blue/green deployments and choose correct strategy for a scenario.
- Define explicit rollback triggers and verification signals (error rate/latency/health).
- Complete Lab 01 with evidence:
- Complete Lab 02 with evidence:

Most important troubleshooting scenarios (top 5):
- Wrong Version Deployed
- Rollout Stuck (Readiness Failures)
- Canary Unhealthy but Stable OK
- Service 503 After Deploy (Endpoints Empty)
- Rollback Didn’t Fix Incident

Practical exam tasks (summary):
- Rollout + Rollback Drill
- Canary Evaluation and Promotion
- Runbook + ADR

Estimated effort level: High — Involves multiple subsystems, failure modes, and operational writing deliverables.

## 08-iac

Learning outcomes:
- Explain Terraform’s mental model: configuration → plan → apply → state.
- Manage state safely: locking, drift detection, and recovery from common state mistakes.
- Design reusable modules with clear inputs/outputs and safe defaults.
- Perform safe changes: small diffs, reversible actions, and explicit verification.
- Troubleshoot Terraform failures with evidence (state, provider errors, dependency graphs).
- Write runbooks and ADRs for IaC decisions (state backend choice, module layout, drift policy).

Files included:
- 00-overview.md
- 01-terraform-mental-model.md
- 02-state-locking-and-drift.md
- 03-modules-and-composition.md
- 04-safe-changes-and-lifecycle.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-local-state-plan-apply-destroy.md
- lab-02-drift-and-state-recovery.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-local-state-plan-apply-destroy.md: local-first drill with verification + cleanup evidence
- lab-02-drift-and-state-recovery.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain Terraform core loop and why state matters.
- Demonstrate safe hygiene:
- Demonstrate state safety:
- Complete Lab 01 with evidence (init/plan/apply/destroy).
- Complete Lab 02 with evidence:

Most important troubleshooting scenarios (top 5):
- Wrong Directory / Wrong Environment
- Drift Detected
- Provider Download Fails
- Unexpected Replacement
- State Refactor Needed After Rename

Practical exam tasks (summary):
- Build a Small Local Terraform Stack
- Drift Detection and Reconciliation
- Safe Refactor with state mv
- Runbook + ADR

Estimated effort level: High — Involves multiple subsystems, failure modes, and operational writing deliverables.

## 09-config-mgmt

Learning outcomes:
- Explain idempotency and why it reduces incident risk.
- Build inventories and variables with clear structure (group_vars/host_vars).
- Write safe playbooks with handlers, check mode, and minimal privileges.
- Debug playbook failures using verbose output and target inspection.
- Manage configuration drift and document operational changes.
- Write runbooks and ADRs for fleet management decisions.

Files included:
- 00-overview.md
- 01-idempotency-and-modules.md
- 02-inventory-and-variables.md
- 03-handlers-templates-and-check-mode.md
- 04-operational-safety.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-local-idempotent-playbook.md
- lab-02-drift-and-safe-rollouts.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-local-idempotent-playbook.md: local-first drill with verification + cleanup evidence
- lab-02-drift-and-safe-rollouts.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain idempotency and why it reduces incident risk.
- Build an inventory and demonstrate `--limit` use.
- Use variables with a clear structure (group_vars).
- Use templates and handlers appropriately.
- Demonstrate check mode and diff as a preview gate.

Most important troubleshooting scenarios (top 5):
- Wrong Host Targeted
- Variables Overridden Unexpectedly
- Playbook Not Idempotent
- Handler Never Runs
- Handler Runs Every Time

Practical exam tasks (summary):
- Idempotent Playbook
- Check Mode Gate
- Safe Rollout Controls
- Runbook + ADR

Estimated effort level: Medium — Requires two labs plus troubleshooting drills, rubric scoring, and a practical exam.

## 10-security

Learning outcomes:
- Apply threat modeling basics to operational systems (not just code).
- Handle secrets safely across local dev, CI, and runtime (no leaks in logs/repos).
- Implement practical security guardrails in delivery pipelines (policy gates).
- Understand supply-chain basics: dependencies, base images, SBOM mindset, provenance intent.
- Debug common security incidents: leaked token, compromised dependency, unsafe permissions, misconfigured network exposure.
- Write runbooks and ADRs for security controls and tradeoffs.

Files included:
- 00-overview.md
- 01-threat-modeling-for-operators.md
- 02-secrets-hygiene.md
- 03-policy-gates-and-least-privilege.md
- 04-supply-chain-basics.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-secret-leak-prevention-git-hook.md
- lab-02-ci-security-gates-manifest.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-secret-leak-prevention-git-hook.md: local-first drill with verification + cleanup evidence
- lab-02-ci-security-gates-manifest.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Write a simple threat model for a small service (assets, entry points, trust boundaries).
- Demonstrate secrets hygiene:
- Implement a baseline guardrail:
- Complete Lab 01 (git hook blocks secret-like commits).
- Complete Lab 02 (policy gate script with at least 2 checks).

Most important troubleshooting scenarios (top 5):
- Secret Committed to Repo
- Secret Printed in CI Logs
- :latest Base Image Used
- Dependency Drift
- Over-Permissive CI Token

Practical exam tasks (summary):
- Threat Model a Small Service
- Secrets Guardrail
- Supply Chain Baseline Gate
- Runbook + ADR

Estimated effort level: High — Involves multiple subsystems, failure modes, and operational writing deliverables.

## 11-observability

Learning outcomes:
- Explain the differences between logs, metrics, and traces and when each is the right tool.
- Design actionable alerts with low noise and clear runbook links.
- Define SLOs and SLIs and connect them to alerting and error budgets.
- Build dashboards as products (audience, questions answered, and drill-down paths).
- Debug outages using observability evidence (not guesses).

Files included:
- 00-overview.md
- 01-logs-metrics-traces.md
- 02-alerting-design.md
- 03-slos-slIs-error-budgets.md
- 04-dashboards-and-debugging.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-instrument-a-service-local.md
- lab-02-alerts-and-runbooks-drill.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-instrument-a-service-local.md: local-first drill with verification + cleanup evidence
- lab-02-alerts-and-runbooks-drill.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain logs vs metrics vs traces with concrete examples.
- Demonstrate structured logging and avoidance of secrets in logs.
- Design 2 actionable alerts (availability + latency) with runbook links.
- Define an SLI/SLO and compute error budget conceptually.
- Design a dashboard outline with drill-down paths.

Most important troubleshooting scenarios (top 5):
- Alert Fires With No Runbook
- High Error Rate After Deploy
- Latency Spike Without Errors
- Retry Storm
- Metrics Cardinality Explosion

Practical exam tasks (summary):
- Instrumentation
- Alert Design
- SLO Definition
- Runbook + ADR

Estimated effort level: High — Involves multiple subsystems, failure modes, and operational writing deliverables.

## 12-sre

Learning outcomes:
- Run incident response with clear roles, timelines, and evidence-based decisions.
- Use SLOs and error budgets to prioritize reliability work vs feature delivery.
- Identify and reduce toil with automation and better systems design.
- Perform capacity planning at a practical level and avoid common scaling failure modes.
- Write postmortems that produce prevention work (not blame).

Files included:
- 00-overview.md
- 01-incident-management.md
- 02-error-budgets-and-risk.md
- 03-toil-and-automation.md
- 04-capacity-and-resilience.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-incident-drill-and-postmortem.md
- lab-02-slo-error-budget-policy.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-incident-drill-and-postmortem.md: local-first drill with verification + cleanup evidence
- lab-02-slo-error-budget-policy.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Run an incident drill with roles, timeline, and evidence.
- Demonstrate containment vs fix discipline.
- Write a postmortem with concrete prevention actions.
- Define an SLO and an error budget policy that changes release behavior.
- Identify and quantify at least 3 sources of toil and propose automation.

Most important troubleshooting scenarios (top 5):
- No IC, Everyone “Doing Stuff”
- Rollback vs Forward-Fix Decision
- Error Budget Exhausted
- Alert Storm
- Toil Spike

Practical exam tasks (summary):
- Incident Drill
- Postmortem
- SLO + Error Budget Policy
- Runbook + ADR

Estimated effort level: High — Involves multiple subsystems, failure modes, and operational writing deliverables.

## 13-programming-for-devops

Learning outcomes:
- Write small, reliable automation tools (CLI scripts) with good failure behavior.
- Use APIs safely (timeouts, retries with backoff, pagination, rate limits).
- Parse and generate structured data (JSON/YAML) without brittle grep pipelines.
- Design scripts that are idempotent and safe to rerun in production.
- Add minimal tests and reproducible execution (Make targets).

Files included:
- 00-overview.md
- 01-reliability-in-scripts.md
- 02-http-apis-and-resilience.md
- 03-data-parsing-and-formats.md
- 04-testing-and-packaging.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-build-a-safe-cli-tool.md
- lab-02-api-client-retries-and-pagination.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-build-a-safe-cli-tool.md: local-first drill with verification + cleanup evidence
- lab-02-api-client-retries-and-pagination.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Build a CLI tool with safe defaults (dry-run) and bounded destructive actions.
- Use timeouts and retries with backoff for API calls.
- Handle pagination and max-items bounds.
- Emit structured output (JSON) for tooling integration.
- Avoid secrets in logs and stdout.

Most important troubleshooting scenarios (top 5):
- Runaway Script (No Bounds)
- Tool Hangs
- Retry Storm Against API
- Parsing Breaks After API Change
- Duplicate Writes

Practical exam tasks (summary):
- Safe CLI Tool
- Resilient API Client
- Runbook + ADR

Estimated effort level: Medium — Requires two labs plus troubleshooting drills, rubric scoring, and a practical exam.

## 14-web-proxy

Learning outcomes:
- Explain reverse proxy responsibilities: routing, TLS termination, buffering, timeouts, retries.
- Configure a basic reverse proxy safely (headers, health checks, upstreams).
- Diagnose common production failures: 502/503/504, header issues, websocket upgrades, timeouts, connection reuse.
- Understand proxy-related security edges: host header injection, auth forwarding, request smuggling basics.
- Write runbooks and ADRs for proxy policies (timeouts, buffering, header forwarding).

Files included:
- 00-overview.md
- 01-reverse-proxy-mental-model.md
- 02-timeouts-buffering-and-retries.md
- 03-headers-auth-and-client-ip.md
- 04-troubleshooting-502-503-504.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-nginx-reverse-proxy-local.md
- lab-02-break-fix-timeouts-and-headers.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-nginx-reverse-proxy-local.md: local-first drill with verification + cleanup evidence
- lab-02-break-fix-timeouts-and-headers.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain reverse proxy responsibilities and failure domains.
- Explain 502 vs 503 vs 504 with common root causes.
- Configure a basic reverse proxy (routing + headers).
- Explain and justify timeout and buffering settings for a service.
- Complete Lab 01 and Lab 02 with evidence and verification.

Most important troubleshooting scenarios (top 5):
- 502 After Deploy
- 504 Under Load
- 503 No Healthy Upstreams
- Wrong Host Routing
- Client IP Lost

Practical exam tasks (summary):
- Configure a Reverse Proxy
- Break/Fix Drill
- Runbook + ADR

Estimated effort level: High — Involves multiple subsystems, failure modes, and operational writing deliverables.

## 15-networking-protocols

Learning outcomes:
- Explain and debug TCP vs UDP failure patterns (refused vs timeout vs reset).
- Diagnose DNS issues (resolution, caching, TTL, split-horizon, NXDOMAIN).
- Debug HTTP behaviors (status codes, redirects, keepalive, proxies).
- Debug TLS problems (SNI, cert chains, time skew, handshake failures).
- Use a consistent network triage playbook with evidence and minimal assumptions.

Files included:
- 00-overview.md
- 01-tcp-udp-basics-and-failure-patterns.md
- 02-dns-basics-and-debugging.md
- 03-http-behavior-and-debugging.md
- 04-tls-basics-and-debugging.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-sockets-and-connectivity-drill.md
- lab-02-dns-and-tls-debug-drill.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-sockets-and-connectivity-drill.md: local-first drill with verification + cleanup evidence
- lab-02-dns-and-tls-debug-drill.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain refused vs timeout vs reset and what each implies.
- Demonstrate a repeatable network triage flow (listener → DNS → connect → TLS → HTTP).
- Debug DNS resolution issues and explain TTL/caching implications.
- Debug TLS handshake issues without disabling verification.
- Complete Lab 01 and Lab 02 with evidence.

Most important troubleshooting scenarios (top 5):
- Connection Refused
- Timeout (Firewall/Route)
- DNS NXDOMAIN
- DNS SERVFAIL / Resolver Down
- Split-Horizon Surprise

Practical exam tasks (summary):
- Failure Classification Drill
- DNS Debug
- TLS Debug
- Runbook + ADR

Estimated effort level: High — Involves multiple subsystems, failure modes, and operational writing deliverables.

## 16-serverless

Learning outcomes:
- Explain the operational model and failure modes of this domain.
- Run local-first drills with verification signals and cleanups.
- Write an actionable runbook and one decision record (ADR).
- Debug common incidents using a consistent triage flow.

Files included:
- 00-overview.md
- 01-event-model-and-triggers.md
- 02-cold-starts-and-performance.md
- 03-concurrency-and-backpressure.md
- 04-observability-and-debugging.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-local-event-processor.md
- lab-02-retries-and-dead-letter.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-local-event-processor.md: local-first drill with verification + cleanup evidence
- lab-02-retries-and-dead-letter.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain the core mental model and 3 common failure patterns.
- Complete Lab 01 and Lab 02 with evidence and cleanup proof.
- Complete at least 12 scenarios from troubleshooting-lab.md.
- Complete exam.md with reasoning and post-incident follow-ups.
- Write one runbook and one ADR that another engineer could execute.

Most important troubleshooting scenarios (top 5):

Practical exam tasks (summary):
- Design a small system for this domain and document assumptions.
- Demonstrate a controlled failure injection and safe recovery.
- Produce a runbook section that covers detection → diagnosis → mitigation → follow-ups.
- Write one ADR about a key tradeoff (latency vs safety, cost vs reliability, etc.).

Estimated effort level: Medium — Requires two labs plus troubleshooting drills, rubric scoring, and a practical exam.

## 17-artifacts

Learning outcomes:
- Explain the operational model and failure modes of this domain.
- Run local-first drills with verification signals and cleanups.
- Write an actionable runbook and one decision record (ADR).
- Debug common incidents using a consistent triage flow.

Files included:
- 00-overview.md
- 01-artifact-identity-and-digests.md
- 02-sbom-basics.md
- 03-signing-and-attestations.md
- 04-promotion-and-retention.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-pin-and-promote-by-digest.md
- lab-02-generate-and-verify-sbom.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-pin-and-promote-by-digest.md: local-first drill with verification + cleanup evidence
- lab-02-generate-and-verify-sbom.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain the core mental model and 3 common failure patterns.
- Complete Lab 01 and Lab 02 with evidence and cleanup proof.
- Complete at least 12 scenarios from troubleshooting-lab.md.
- Complete exam.md with reasoning and post-incident follow-ups.
- Write one runbook and one ADR that another engineer could execute.

Most important troubleshooting scenarios (top 5):

Practical exam tasks (summary):
- Design a small system for this domain and document assumptions.
- Demonstrate a controlled failure injection and safe recovery.
- Produce a runbook section that covers detection → diagnosis → mitigation → follow-ups.
- Write one ADR about a key tradeoff (latency vs safety, cost vs reliability, etc.).

Estimated effort level: Medium — Requires two labs plus troubleshooting drills, rubric scoring, and a practical exam.

## 18-service-mesh

Learning outcomes:
- Explain the operational model and failure modes of this domain.
- Run local-first drills with verification signals and cleanups.
- Write an actionable runbook and one decision record (ADR).
- Debug common incidents using a consistent triage flow.

Files included:
- 00-overview.md
- 01-when-to-use-a-mesh.md
- 02-mtls-and-identity.md
- 03-traffic-policy-and-resilience.md
- 04-failure-domains-and-debugging.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-mtls-between-two-services.md
- lab-02-timeouts-retries-and-outages.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-mtls-between-two-services.md: local-first drill with verification + cleanup evidence
- lab-02-timeouts-retries-and-outages.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain the core mental model and 3 common failure patterns.
- Complete Lab 01 and Lab 02 with evidence and cleanup proof.
- Complete at least 12 scenarios from troubleshooting-lab.md.
- Complete exam.md with reasoning and post-incident follow-ups.
- Write one runbook and one ADR that another engineer could execute.

Most important troubleshooting scenarios (top 5):

Practical exam tasks (summary):
- Design a small system for this domain and document assumptions.
- Demonstrate a controlled failure injection and safe recovery.
- Produce a runbook section that covers detection → diagnosis → mitigation → follow-ups.
- Write one ADR about a key tradeoff (latency vs safety, cost vs reliability, etc.).

Estimated effort level: Medium — Requires two labs plus troubleshooting drills, rubric scoring, and a practical exam.

## 19-email

Learning outcomes:
- Explain the operational model and failure modes of this domain.
- Run local-first drills with verification signals and cleanups.
- Write an actionable runbook and one decision record (ADR).
- Debug common incidents using a consistent triage flow.

Files included:
- 00-overview.md
- 01-smtp-mental-model.md
- 02-deliverability-spf-dkim-dmarc.md
- 03-bounces-retries-and-queues.md
- 04-debugging-headers-and-traces.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-smtp-session-drill.md
- lab-02-trace-a-delivery-failure.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-smtp-session-drill.md: local-first drill with verification + cleanup evidence
- lab-02-trace-a-delivery-failure.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain the core mental model and 3 common failure patterns.
- Complete Lab 01 and Lab 02 with evidence and cleanup proof.
- Complete at least 12 scenarios from troubleshooting-lab.md.
- Complete exam.md with reasoning and post-incident follow-ups.
- Write one runbook and one ADR that another engineer could execute.

Most important troubleshooting scenarios (top 5):

Practical exam tasks (summary):
- Design a small system for this domain and document assumptions.
- Demonstrate a controlled failure injection and safe recovery.
- Produce a runbook section that covers detection → diagnosis → mitigation → follow-ups.
- Write one ADR about a key tradeoff (latency vs safety, cost vs reliability, etc.).

Estimated effort level: Medium — Requires two labs plus troubleshooting drills, rubric scoring, and a practical exam.

## 20-datastores-for-devops

Learning outcomes:
- Explain the operational model and failure modes of this domain.
- Run local-first drills with verification signals and cleanups.
- Write an actionable runbook and one decision record (ADR).
- Debug common incidents using a consistent triage flow.

Files included:
- 00-overview.md
- 01-postgres-ops-baseline.md
- 02-redis-ops-baseline.md
- 03-backups-restore-and-dr.md
- 04-performance-and-locks.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-local-postgres-backup-restore.md
- lab-02-redis-failure-drill.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-local-postgres-backup-restore.md: local-first drill with verification + cleanup evidence
- lab-02-redis-failure-drill.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain the core mental model and 3 common failure patterns.
- Complete Lab 01 and Lab 02 with evidence and cleanup proof.
- Complete at least 12 scenarios from troubleshooting-lab.md.
- Complete exam.md with reasoning and post-incident follow-ups.
- Write one runbook and one ADR that another engineer could execute.

Most important troubleshooting scenarios (top 5):

Practical exam tasks (summary):
- Design a small system for this domain and document assumptions.
- Demonstrate a controlled failure injection and safe recovery.
- Produce a runbook section that covers detection → diagnosis → mitigation → follow-ups.
- Write one ADR about a key tradeoff (latency vs safety, cost vs reliability, etc.).

Estimated effort level: High — Involves multiple subsystems, failure modes, and operational writing deliverables.

## 21-message-queues-streaming

Learning outcomes:
- Explain the operational model and failure modes of this domain.
- Run local-first drills with verification signals and cleanups.
- Write an actionable runbook and one decision record (ADR).
- Debug common incidents using a consistent triage flow.

Files included:
- 00-overview.md
- 01-delivery-semantics-and-idempotency.md
- 02-retries-dlq-and-poison-messages.md
- 03-ordering-partitions-and-throughput.md
- 04-lag-backpressure-and-triage.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-build-a-local-queue.md
- lab-02-duplicate-delivery-drill.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-build-a-local-queue.md: local-first drill with verification + cleanup evidence
- lab-02-duplicate-delivery-drill.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain the core mental model and 3 common failure patterns.
- Complete Lab 01 and Lab 02 with evidence and cleanup proof.
- Complete at least 12 scenarios from troubleshooting-lab.md.
- Complete exam.md with reasoning and post-incident follow-ups.
- Write one runbook and one ADR that another engineer could execute.

Most important troubleshooting scenarios (top 5):

Practical exam tasks (summary):
- Design a small system for this domain and document assumptions.
- Demonstrate a controlled failure injection and safe recovery.
- Produce a runbook section that covers detection → diagnosis → mitigation → follow-ups.
- Write one ADR about a key tradeoff (latency vs safety, cost vs reliability, etc.).

Estimated effort level: High — Involves multiple subsystems, failure modes, and operational writing deliverables.

## 22-identity-and-access

Learning outcomes:
- Explain the operational model and failure modes of this domain.
- Run local-first drills with verification signals and cleanups.
- Write an actionable runbook and one decision record (ADR).
- Debug common incidents using a consistent triage flow.

Files included:
- 00-overview.md
- 01-oidc-oauth-mental-model.md
- 02-tokens-claims-and-expiry.md
- 03-rbac-and-policy-debugging.md
- 04-workload-identity-patterns.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-jwt-signing-and-validation.md
- lab-02-rbac-failure-drill.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-jwt-signing-and-validation.md: local-first drill with verification + cleanup evidence
- lab-02-rbac-failure-drill.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain the core mental model and 3 common failure patterns.
- Complete Lab 01 and Lab 02 with evidence and cleanup proof.
- Complete at least 12 scenarios from troubleshooting-lab.md.
- Complete exam.md with reasoning and post-incident follow-ups.
- Write one runbook and one ADR that another engineer could execute.

Most important troubleshooting scenarios (top 5):

Practical exam tasks (summary):
- Design a small system for this domain and document assumptions.
- Demonstrate a controlled failure injection and safe recovery.
- Produce a runbook section that covers detection → diagnosis → mitigation → follow-ups.
- Write one ADR about a key tradeoff (latency vs safety, cost vs reliability, etc.).

Estimated effort level: High — Involves multiple subsystems, failure modes, and operational writing deliverables.

## 23-release-engineering

Learning outcomes:
- Explain the operational model and failure modes of this domain.
- Run local-first drills with verification signals and cleanups.
- Write an actionable runbook and one decision record (ADR).
- Debug common incidents using a consistent triage flow.

Files included:
- 00-overview.md
- 01-versioning-and-changelogs.md
- 02-build-once-promote-many.md
- 03-release-gates-and-quality.md
- 04-rollbacks-and-migrations.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-release-metadata-and-promotion.md
- lab-02-rollback-drill.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-release-metadata-and-promotion.md: local-first drill with verification + cleanup evidence
- lab-02-rollback-drill.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain the core mental model and 3 common failure patterns.
- Complete Lab 01 and Lab 02 with evidence and cleanup proof.
- Complete at least 12 scenarios from troubleshooting-lab.md.
- Complete exam.md with reasoning and post-incident follow-ups.
- Write one runbook and one ADR that another engineer could execute.

Most important troubleshooting scenarios (top 5):

Practical exam tasks (summary):
- Design a small system for this domain and document assumptions.
- Demonstrate a controlled failure injection and safe recovery.
- Produce a runbook section that covers detection → diagnosis → mitigation → follow-ups.
- Write one ADR about a key tradeoff (latency vs safety, cost vs reliability, etc.).

Estimated effort level: High — Involves multiple subsystems, failure modes, and operational writing deliverables.

## 24-platform-engineering

Learning outcomes:
- Explain the operational model and failure modes of this domain.
- Run local-first drills with verification signals and cleanups.
- Write an actionable runbook and one decision record (ADR).
- Debug common incidents using a consistent triage flow.

Files included:
- 00-overview.md
- 01-golden-paths-and-templates.md
- 02-guardrails-and-policies.md
- 03-multi-tenancy-and-rbac.md
- 04-idp-concepts-and-adoption.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-build-a-template-repo.md
- lab-02-add-policy-gates.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-build-a-template-repo.md: local-first drill with verification + cleanup evidence
- lab-02-add-policy-gates.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain the core mental model and 3 common failure patterns.
- Complete Lab 01 and Lab 02 with evidence and cleanup proof.
- Complete at least 12 scenarios from troubleshooting-lab.md.
- Complete exam.md with reasoning and post-incident follow-ups.
- Write one runbook and one ADR that another engineer could execute.

Most important troubleshooting scenarios (top 5):

Practical exam tasks (summary):
- Design a small system for this domain and document assumptions.
- Demonstrate a controlled failure injection and safe recovery.
- Produce a runbook section that covers detection → diagnosis → mitigation → follow-ups.
- Write one ADR about a key tradeoff (latency vs safety, cost vs reliability, etc.).

Estimated effort level: High — Involves multiple subsystems, failure modes, and operational writing deliverables.

## 25-cost-reliability-finops

Learning outcomes:
- Explain the operational model and failure modes of this domain.
- Run local-first drills with verification signals and cleanups.
- Write an actionable runbook and one decision record (ADR).
- Debug common incidents using a consistent triage flow.

Files included:
- 00-overview.md
- 01-cost-drivers-and-tagging.md
- 02-budgets-guardrails-and-alerts.md
- 03-unit-economics-and-capacity.md
- 04-tradeoffs-cost-vs-reliability.md
- deep-dive-01.md
- deep-dive-02.md
- lab-01-build-a-local-cost-model.md
- lab-02-rightsize-and-rollback.md
- cloud-extension-lab.md
- checklist.md
- rubric.md
- review-questions.md
- exam.md
- common-mistakes.md
- troubleshooting.md
- troubleshooting-lab.md
- runbook-template.md
- decision-record-template.md

Labs included + what they build:
- lab-01-build-a-local-cost-model.md: local-first drill with verification + cleanup evidence
- lab-02-rightsize-and-rollback.md: local-first drill with verification + cleanup evidence

What “done” means (top 5 checklist items):
- Explain the core mental model and 3 common failure patterns.
- Complete Lab 01 and Lab 02 with evidence and cleanup proof.
- Complete at least 12 scenarios from troubleshooting-lab.md.
- Complete exam.md with reasoning and post-incident follow-ups.
- Write one runbook and one ADR that another engineer could execute.

Most important troubleshooting scenarios (top 5):

Practical exam tasks (summary):
- Design a small system for this domain and document assumptions.
- Demonstrate a controlled failure injection and safe recovery.
- Produce a runbook section that covers detection → diagnosis → mitigation → follow-ups.
- Write one ADR about a key tradeoff (latency vs safety, cost vs reliability, etc.).

Estimated effort level: High — Involves multiple subsystems, failure modes, and operational writing deliverables.
