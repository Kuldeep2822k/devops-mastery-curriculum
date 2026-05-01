---
title: Checklist (Definition of Done)
tags:
  - checklist
  - ci
module: "04"
---

# Checklist — Module 04 CI (DoD)

- [ ] Explain a CI pipeline architecture with stages, artifacts, caching, and gates.
- [ ] Define artifact contracts for your pipeline (what is produced/consumed and where).
- [ ] Implement a portable pipeline using Make targets and verify:
  - [ ] `make ci` works locally
  - [ ] `make ci` works in a clean runner container
  - [ ] smoke gate fails when dist/ is missing
- [ ] Implement a GitHub Actions pipeline and verify:
  - [ ] triggers on PR and push
  - [ ] caching keyed safely (lockfile hash/tool version)
  - [ ] artifacts uploaded (dist/)
  - [ ] fail-fast observed on intentional failure
- [ ] Explain and apply safe secrets handling:
  - [ ] environment separation
  - [ ] approvals for high-risk stages
  - [ ] no secrets printed to logs
- [ ] Complete at least 12 scenarios from [troubleshooting-lab.md](troubleshooting-lab.md) with written diagnosis and verification.
- [ ] Complete [exam.md](exam.md) with evidence (commands + expected signals + reasoning).
- [ ] Write a CI runbook using [runbook-template.md](runbook-template.md).
- [ ] Write one ADR about pipeline design (cache keys, artifact layout, fail-fast strategy) using [decision-record-template.md](decision-record-template.md).
- [ ] Self-grade via [rubric.md](rubric.md) and list 3 improvement actions for CI reliability.
