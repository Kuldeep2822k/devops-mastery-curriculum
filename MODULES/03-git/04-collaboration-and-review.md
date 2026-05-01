---
title: Collaboration and Review (Operational Quality)
tags:
  - git
  - collaboration
  - review
module: "03"
---

# Collaboration and Review (Operational Quality)

## PRs Are Operational Controls

Code review is not about style; it prevents incidents by catching:

- risky changes without rollback path
- missing verification
- secrets and unsafe patterns
- changes that increase operational complexity

## Review Checklist (Operator Lens)

- What is the change’s risk level?
- What are the verification signals?
- What is the rollback plan?
- Does it change configs, permissions, or network behavior?
- Does it affect dependencies or data?
- Does it need an ADR?

## Branch Protections (Intent)

Branch protection is not “security theater”. It enforces:

- code review before merge
- required checks (tests, lint, policy)
- protected release tags

Tradeoff:

- stricter protections reduce risk but can slow urgent hotfixes

Design principle:

- keep a documented, audited emergency path rather than removing protections entirely

## Handling Hotfixes Without Chaos

Operationally mature approach:

- hotfix branches from a release tag or main
- minimal diff
- same pipeline path as normal merges
- immediate release tag
- post-incident follow-up PR to merge hotfix back into main if needed

## Anti-Patterns

- approving without understanding impact
- merging red builds
- bypassing protections casually
- “LGTM” without verifying rollback or tests
