# DevOps From Zero to Staff: The Complete Local-First + Cloud-Ready Master Guide

A production-grade, local-first learning system to build real operational competence: delivery systems, production operations, incident response under pressure, reliability engineering, security/supply-chain hygiene, and staff-level tradeoff thinking.

This repo is designed to be used like an engineering program: you produce evidence, run drills, write runbooks, and practice safe rollback and verification.

## What “Competence” Means Here

You finish each module with evidence:

- You can build, break, and fix systems locally (repeatably).
- You can explain failures from symptoms → diagnosis commands → root cause → fix → prevention.
- You can write runbooks and decision records (ADRs) that another engineer can execute.
- You can sit an on-call drill (time-boxed) and produce an incident log + follow-ups.

## Getting Started

```bash
git clone https://github.com/Kuldeep2822k/devops-mastery-curriculum.git devops-staff-guide
cd DEVOPS_STAFF_MASTER_GUIDE
```

- Start here: [ROADMAP.md](ROADMAP.md)
- Read the operating model: [00-how-to-use.md](00-HOW-TO-USE/00-how-to-use.md)
- Bootstrap tooling: [SETUP/00-overview.md](SETUP/00-overview.md)
- Pick a schedule: [SCHEDULES/](SCHEDULES/)

## Local-First, Cloud-Ready

Core labs run locally using safe, reproducible primitives:

- Containers: Docker
- Kubernetes: kind or minikube
- IaC: Terraform/OpenTofu (local-first workflow; cloud extensions optional)
- CI: local pipeline simulation + GitHub Actions-style contracts
- Observability: local signals (logs + metrics) with optional stack extensions

Cloud usage is optional and always isolated to:

- per-module `cloud-extension-lab.md`
- provider mappings and notes under [APPENDICES/](APPENDICES/)

Every cloud extension includes cost control and cleanup guidance. Do not run cloud labs without reading [03-lab-safety-cost-control.md](00-HOW-TO-USE/03-lab-safety-cost-control.md).

## Repository Layout

- Core navigation:
  - Roadmap and progression: [ROADMAP.md](ROADMAP.md)
  - How to use: [00-HOW-TO-USE/](00-HOW-TO-USE/)
  - Setup/tooling: [SETUP/](SETUP/)
- Curriculum:
  - Modules (01–25): [MODULES/](MODULES/)
  - Troubleshooting catalog: [TROUBLESHOOTING_CATALOG/](TROUBLESHOOTING_CATALOG/)
  - Cheatsheets: [CHEATSHEETS/](CHEATSHEETS/)
- Portfolio:
  - Projects (01–10): [PROJECTS/00-overview.md](PROJECTS/00-overview.md)
  - Capstones (01–04): [CAPSTONES/00-overview.md](CAPSTONES/00-overview.md)
- Hiring practice:
  - Interview drills: [INTERVIEW_DRILLS/](INTERVIEW_DRILLS/)
- Tracking:
  - Progress templates: [PROGRESS/](PROGRESS/)

## Evidence You Produce

- Completed checklist.md (Definition of Done)
- Self-score via rubric.md (0–4 per skill)
- Notes from review-questions.md
- Exam submissions (commands run + outputs + reasoning)
- A runbook draft + an ADR draft (even for “simple” modules)
- Postmortem + follow-ups for on-call drills

Evidence guidance: [04-evidence-rubrics.md](00-HOW-TO-USE/04-evidence-rubrics.md)

## How To Navigate a Module

Each module is a complete mini-course with:

- overview + concept lessons (`01-*.md`, `02-*.md`, …)
- runnable labs (`lab-*.md`) with strict sections: Goal/Prereqs/Setup/Steps/Verify/Cleanup/Troubleshooting
- assessment artifacts (checklist, rubric, review questions, exam)
- troubleshooting practice (`troubleshooting.md` and `troubleshooting-lab.md`)

## Safety and Security Rules (Non-Negotiable)

- Never paste secrets into terminals, files, screenshots, or logs.
- Prefer short-lived credentials and workload identity patterns.
- Use least privilege; design for revocation.
- Treat build artifacts as supply-chain inputs: provenance, SBOM, signatures where feasible.
- Always include cleanup steps and verify resource deletion.

Safety and cost control: [03-lab-safety-cost-control.md](00-HOW-TO-USE/03-lab-safety-cost-control.md)

## Recommended Progression

Pick one schedule and follow it:

- [SCHEDULES/30-day-boot.md](SCHEDULES/30-day-boot.md)
- [SCHEDULES/90-day-deep.md](SCHEDULES/90-day-deep.md)
- [SCHEDULES/180-day-staff.md](SCHEDULES/180-day-staff.md)
- [SCHEDULES/365-day-mastery.md](SCHEDULES/365-day-mastery.md)

## QA

- QA checklist: [QA_AUDIT.md](QA_AUDIT.md)
- Self-audit report template: [QA_SELF_AUDIT_REPORT.md](QA_SELF_AUDIT_REPORT.md)
- Why the guide uses small Python tools: [WHY_PY_TOOLS.md](WHY_PY_TOOLS.md)

## Generated Files Manifest

Track generated content and repository status in [GENERATED_FILES_MANIFEST.md](GENERATED_FILES_MANIFEST.md).
