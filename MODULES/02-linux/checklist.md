---
title: Checklist (Definition of Done)
tags:
  - checklist
  - linux
module: "02"
---

# Checklist — Module 02 Linux (DoD)

- [ ] Perform a 5-minute Linux triage and explain what each command reveals (CPU/mem/disk/network).
- [ ] Explain process lifecycle, signals, and safe termination (TERM vs KILL).
- [ ] Demonstrate file permission reasoning:
  - [ ] identify owner/group/mode for a file
  - [ ] explain why a process can’t read/write a path (including directory traversal)
- [ ] Demonstrate systemd/journald competence:
  - [ ] inspect a unit (ExecStart, User, Restart)
  - [ ] read logs for the last 15 minutes and for current boot
- [ ] Complete Lab 01:
  - [ ] create a user systemd service
  - [ ] break and fix ExecStart path issue using evidence
  - [ ] break and fix a port collision
  - [ ] cleanup verified (unit removed, port not listening)
- [ ] Complete Lab 02:
  - [ ] establish baseline
  - [ ] induce CPU pressure and recover
  - [ ] induce disk pressure (bounded) and recover
  - [ ] simulate memory pressure safely and recover
- [ ] Complete at least 12 scenarios from [troubleshooting-lab.md](troubleshooting-lab.md) with written diagnosis and verification.
- [ ] Complete [exam.md](exam.md) with evidence artifacts.
- [ ] Write a runbook draft using [runbook-template.md](runbook-template.md).
- [ ] Write one ADR using [decision-record-template.md](decision-record-template.md) about an operational change (restart policy, limits, unit design).
- [ ] Self-grade via [rubric.md](rubric.md) and list 2 concrete improvement actions.
