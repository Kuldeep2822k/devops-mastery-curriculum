---
title: Reviewer Guide
tags:
  - reviewer
  - overview
---

# DevOps From Zero to Staff — Reviewer Guide

## 1) High-level summary (1 page max)

What this guide is:
- A repository-style, Obsidian-friendly DevOps curriculum designed to produce operational competence via repeatable labs, incident drills, and evidence artifacts.
- Audience: learners and teams onboarding into on-call engineering roles; suitable for self-study or internal enablement.
- Outcomes: ability to build, break, diagnose, recover, and document real systems with safe defaults.

What makes it different:
- Local-first: core labs are intended to run without a cloud account; cloud work is optional and isolated to cloud-extension labs.
- Cloud-agnostic core: concepts focus on invariants (state, rollbacks, identity, observability) rather than provider specifics.
- Evidence-based: completion requires checklists, rubric scoring, exam submissions, and written runbooks/ADRs.
- Troubleshooting-driven: every module includes a troubleshooting-lab with time-boxed incident scenarios.

How a learner should progress through it:
- Core modules (01–12) build operator fundamentals and SRE foundations.
- Expansion modules (13–19) add breadth across real production systems (networking, proxies, serverless, artifacts, mesh, email).
- Staff depth modules (20–25) add datastores, messaging, identity, release engineering, platform engineering, and FinOps tradeoffs.
- Projects integrate multiple modules; capstones force staff-level ambiguity and tradeoff writing.

Key operating model:
- The “Core Loop” is defined in 00-HOW-TO-USE/00-how-to-use.md (concepts → deep dives → labs → troubleshooting drill → checklist/rubric/exam → runbook/ADR).

## 2) Repository manifest (exhaustive)

This repo includes two reviewer-oriented artifacts so you do not need to open every curriculum file:
- REVIEWER_REPO_TREE.txt: complete directory tree including every file.
- REVIEWER_REPO_MANIFEST.md: exhaustive per-file manifest using the schema:
  - path:
  - type:
  - purpose:
  - prereqs:
  - outputs/evidence:

## 3) Module-by-module map (01 → 25)

- REVIEWER_MODULE_MAP.md summarizes each module’s outcomes, included files, labs, Definition of Done, top troubleshooting scenarios, exam tasks, and effort level.

## 4) Cross-cutting systems (how the guide stays consistent)

Standard lab template:
- Labs follow a consistent structure: Goal → Steps → Verify → Cleanup → Evidence.

Debugging framework:
- Symptom → establish ground truth → hypotheses → minimal fix/rollback → verify and monitor → prevent recurrence.
- See 00-HOW-TO-USE/05-troubleshooting-framework.md.

Rubric scoring (0–4):
- A uniform 0–4 scale is defined in 00-HOW-TO-USE/04-evidence-rubrics.md and reused per module.

Progress tracking and repetition:
- PROGRESS/learning-checklist.md tracks completion by module, projects, and capstones.
- PROGRESS/spaced-repetition.md suggests repeating modules at 1 day / 1 week / 1 month intervals.

Cloud extensions rule and cost controls:
- Cloud labs are optional and must follow cost/safety guardrails in 00-HOW-TO-USE/03-lab-safety-cost-control.md.

## 5) How to use this guide — quick start (actionable)

Day 0 setup checklist:
- Read 00-HOW-TO-USE/03-lab-safety-cost-control.md and choose an isolated workspace for labs/evidence.
- Follow SETUP/00-overview.md and the rest of SETUP/ to install required tooling.

Week 1 plan:
- Module 01: read concepts 01–04, then deep dives, then run lab-01 and lab-02 with evidence.
- Run one time-boxed scenario from troubleshooting-lab.md and write an incident log.
- Complete checklist.md, self-score rubric.md, and write one runbook + one ADR using module templates.

How to take notes:
- Use 00-HOW-TO-USE/06-notes-template.md in a separate evidence repo/folder.

How to store evidence:
- Follow the evidence folder convention shown in 00-HOW-TO-USE/00-how-to-use.md and 00-HOW-TO-USE/04-evidence-rubrics.md.

How to recover if stuck:
- Start with module troubleshooting.md and troubleshooting-lab.md, then consult TROUBLESHOOTING_CATALOG/00-index.md for cross-module patterns.

## 6) Verification instructions (critical)

Repository completeness checks:
- Confirm each module directory contains: 00-overview, 4 concepts, 2 deep dives, 2 labs, cloud-extension, checklist, rubric, review questions, exam, troubleshooting, troubleshooting-lab, and runbook/ADR templates.
- Confirm top-level directories exist: SCHEDULES, PROJECTS, CAPSTONES, TROUBLESHOOTING_CATALOG, CHEATSHEETS, INTERVIEW_DRILLS, PROGRESS, ADVANCED, APPENDICES.

Missing files detection (conceptual):
- Enumerate expected files for each module and diff against the actual directory listing.

Link sanity checks:
- Search for Markdown links and ensure referenced local targets exist; avoid assuming code fences are links.

Minimum smoke test (curriculum):
- Pick one lab per module and confirm the steps are runnable locally in principle (inputs, verify signals, cleanup verification).

## 7) Navigation aids

Top-level entry points:
- ROADMAP.md: curriculum progression and cross-links.
- 00-HOW-TO-USE/: operating model, evidence standards, troubleshooting framework.
- SETUP/: workstation and local-first tool bootstrap.
- MODULES/: main curriculum (01–25).
- PROJECTS/ and CAPSTONES/: integration work and staff-level drills.

If you want X, go here:
- Learn CI fast: MODULES/04-ci/
- Kubernetes debugging: MODULES/06-kubernetes/ and TROUBLESHOOTING_CATALOG/02-kubernetes.md
- Terraform state and drift: MODULES/08-iac/
- SRE incident response: MODULES/12-sre/ and 00-HOW-TO-USE/05-troubleshooting-framework.md
- Proxy 502/503/504 diagnosis: MODULES/14-web-proxy/
- DNS/TLS debugging: MODULES/15-networking-protocols/

Recommended paths:
- Core path: Modules 01–12, then Project 01–03.
- Deep path: Modules 01–25, all projects, then capstones, with repetition cycles.
- Cloud path: complete local-first modules first, then selectively run cloud-extension-lab.md pages.

## 8) Quality audit

10 strongest parts:
- Consistent per-module structure (overview → concepts → labs → assessments).
- Evidence-first completion (checklist, rubric, exam, runbook, ADR).
- Troubleshooting framework and time-boxed incident drills across modules.
- Local-first and cost/safety rules are explicitly documented.
- Curriculum progression covers fundamentals, breadth, and staff-level depth.
- Dedicated troubleshooting catalog, cheatsheets, and interview drills for recall.
- Projects and capstones enforce integration and tradeoff thinking.
- Navigation manifest provides a single authoritative list of generated content.
- Notes templates standardize learner artifacts and make review feasible.
- QA self-audit verifies structure and link integrity.

10 biggest risks/gaps:
- Some labs are intentionally generic templates and may require the learner to implement missing scaffolding (by design, but increases onboarding friction).
- Not every lab includes concrete code/assets (Dockerfiles, compose files) inside the guide; evidence is expected to live outside the repo.
- Provider-specific cloud mapping is lightweight and may need deeper provider examples for certain teams.
- Effort levels vary by learner environment (OS differences, tool versions).
- Long lists of per-file links in the manifest can be noisy for human scanning without a rendered view.
- Some advanced domains (service mesh, artifacts signing) benefit from concrete tool walkthroughs that may be added later.
- Projects/capstones specify deliverables but do not include starter code by default.
- Interview drills are high-level prompts and may need scoring rubrics for hiring loops.
- Troubleshooting catalog scenarios are templates and may need expansion to match your stack.
- No automated test harness exists to execute labs end-to-end in CI (intentionally local-first).

Concrete improvements (exact file additions):
- Add PROJECTS/project-01-delivery-pipeline/starter/ (starter files) if you want guided builds inside the repo.
- Add MODULES/17-artifacts/lab-assets/ with sample SBOM/signing scripts if you want concrete tooling examples.
- Add INTERVIEW_DRILLS/rubric.md if you want consistent scoring for interview practice.

References:
- See REVIEWER_REPO_MANIFEST.md for the exhaustive per-file manifest.
- See REVIEWER_MODULE_MAP.md for module-by-module outcomes, labs, and completion criteria.
