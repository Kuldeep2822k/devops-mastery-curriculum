---
title: Idempotency and Modules
tags:
  - ansible
  - idempotency
module: "09"
---

# Idempotency and Modules

## Idempotency (Operator Definition)

An operation is idempotent if:

- running it once or ten times results in the same end state

Why operators care:

- reduces risk during incidents
- makes automation safe to re-run

Anti-pattern:

- scripts that append lines repeatedly, overwrite configs unsafely, or restart services unnecessarily

## Modules vs Shell Commands

Prefer Ansible modules over raw shell:

- modules encode idempotency and return structured results
- shell often requires extra checks and is harder to reason about

Examples of safer modules:

- `file`, `copy`, `template`
- `lineinfile` (use carefully)
- `service` (or `systemd`)

## Handlers and Change Control

Handlers run only when notified by a change.

Operator benefit:

- avoids unnecessary restarts
- reduces service disruption

## Check Mode (Preview)

`--check` lets you preview what would change.

It is not perfect, but it is a strong safety gate.

## Anti-Patterns

- using `shell` for everything
- restarting services on every run
- no dry-run / check-mode workflow
