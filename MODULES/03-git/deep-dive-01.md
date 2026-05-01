---
title: "Deep Dive 01: Rewrite vs Revert (Safety, Trust, and Forensics)"
tags:
  - git
  - deep-dive
  - tradeoffs
module: "03"
---

# Deep Dive 01 — Rewrite vs Revert (Safety, Trust, and Forensics)

## The Core Tension

Two desires conflict:

- Clean history (rebase, squash, rewrite)
- Reliable forensics and collaboration (immutable shared history)

Staff-level judgment is choosing which matters more in a given repo and documenting the policy.

## When Rewriting Is Appropriate

Rewrite is appropriate when:

- the branch is private (no one else has pulled it)
- you are preparing a PR and want to clean up commits for review
- you are removing accidental commits before publication (not secrets; secrets require rotation)

Rules:

- never rewrite shared branches casually
- if you must, coordinate explicitly and provide recovery instructions

## When Reverting Is Preferable

Revert is preferable when:

- the branch is shared or protected
- the change is already deployed and you need a clear audit trail
- incident response requires minimizing coordination risk

Revert preserves:

- history of what happened
- ability to map releases to commits consistently

## Security Note: Secrets Are Special

If a secret is committed:

- treat it as compromised immediately
- rotate/revoke it

History rewriting may still be required to reduce exposure, but it does not remove compromise risk because:

- caches, forks, clones, and logs may already contain it

## Forensics Considerations

Forensics benefits from:

- stable tags
- non-rewritten release branches
- clear commit messages with intent
- small diffs for high-risk changes

## Policy Guidance (Default)

A safe default policy for learning repos:

- allow rebase/squash on feature branches
- disallow force-push to main
- prefer revert for rollback
- tag releases with annotated tags

Capture this in an ADR for your repo.
