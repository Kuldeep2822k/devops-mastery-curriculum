---
title: Decision Record Template (Module 08)
tags:
  - adr
  - template
  - terraform
module: "08"
---

# Decision Record (ADR) — Module 08 Terraform

## Title

ADR-0001: <Terraform Backend / Layout / Drift Policy>

## Status

Proposed | Accepted | Rejected | Superseded

## Context

- What environments exist?
- Team size and apply frequency?
- Security/compliance constraints?
- Need for locking/backups/audit logs?

## Options Considered

### Option A: Local State

- Pros:
- Cons:
- Risks:

### Option B: Remote Backend (with locking)

- Pros:
- Cons:
- Risks:

### Option C: Environment Separation

- Dirs + separate state
- Workspaces

## Decision

- chosen backend:
- env separation approach:
- drift policy:
- apply governance (who/where approvals):

## Consequences

- positive:
- negative:

## Operational Impact

- how we recover state:
- how we prevent concurrent applies:
- runbook requirements:

## Verification Plan

- success signals:
- audit signals:
- rollback triggers:
