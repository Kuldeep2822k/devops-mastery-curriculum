---
title: "Deep Dive 01: Remote State Backends and Team Workflows"
tags:
  - terraform
  - deep-dive
  - state
module: "08"
---

# Deep Dive 01 — Remote State Backends and Team Workflows

## Why Remote State Exists

Remote state backends provide:

- locking (prevent concurrent applies)
- versioning and backups
- centralized state access for teams

Local state is fine for learning, but risky for teams and production.

## Backend Tradeoffs (High Level)

When choosing a backend, consider:

- lock reliability
- access controls and audit logs
- backup/restore guarantees
- latency and availability

## State Access Is Security

State can contain:

- resource IDs
- endpoints
- sometimes secrets (depending on providers)

Security practice:

- restrict access tightly
- encrypt at rest
- avoid exposing state in logs or artifacts

## Team Workflow Standard (Safe Default)

- PR builds a plan (no apply)
- apply happens from a controlled branch/tag with approvals
- one apply at a time enforced by backend lock
- state changes are auditable
