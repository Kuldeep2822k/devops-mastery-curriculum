---
title: Decision Record Template (Module 03)
tags:
  - adr
  - template
  - git
module: "03"
---

# Decision Record (ADR) — Module 03 Git

## Title

ADR-0001: <Repo Workflow Policy>

## Status

Proposed | Accepted | Rejected | Superseded

## Context

- What repo is this for?
- Who are contributors and what is collaboration model?
- What operational requirements exist (auditability, rollback speed, compliance)?

## Options Considered

### Option A: Merge Commits

- Pros:
- Cons:
- Risks:

### Option B: Squash Merges

- Pros:
- Cons:
- Risks:

### Option C: Rebase + Fast-Forward

- Pros:
- Cons:
- Risks:

## Decision

- Chosen option:
- Rules:
  - force-push policy:
  - tagging policy:
  - hotfix policy:

## Consequences

- Positive:
- Negative:

## Operational Impact

- How does rollback work? (revert PR / revert commit / cherry-pick hotfix)
- How do we map artifacts to commits?
- How do we do incident forensics?

## Verification Plan

- checks required before merge:
- how releases are tagged:
- how we audit that policy is followed:
