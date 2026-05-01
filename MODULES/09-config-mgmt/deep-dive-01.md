---
title: "Deep Dive 01: Idempotency Failure Modes and How to Detect Them"
tags:
  - ansible
  - deep-dive
  - idempotency
module: "09"
---

# Deep Dive 01 — Idempotency Failure Modes and How to Detect Them

## Common Idempotency Bugs

- appending lines repeatedly to a file
- using `shell` without guard conditions
- restarting services every run
- generating config with unstable ordering (diffs every run)

## Why Idempotency Matters Under Pressure

During incidents:

- you rerun automation multiple times
- you need predictable outcomes

Non-idempotent automation causes:

- config bloat
- repeated restarts
- harder rollback

## Detection Techniques

- run playbook twice and ensure second run reports “ok” not “changed”
- use `--check` to confirm no changes are needed
- use `--diff` to ensure diffs are stable (no random ordering)

## Prevention Patterns

- prefer modules over shell
- use templates with stable rendering
- use handlers for restarts
- write tests for playbooks (later modules/projects can add this)
