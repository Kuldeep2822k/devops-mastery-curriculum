---
title: Git Mental Model (Objects, Refs, and Safety)
tags:
  - git
  - mental-model
module: "03"
---

# Git Mental Model (Objects, Refs, and Safety)

## What Git Actually Tracks

Git is an object database plus references.

- blobs: file contents
- trees: directory snapshots
- commits: snapshots + metadata + parent pointers
- tags: named references (lightweight or annotated)

Refs are movable pointers:

- branches are refs that move forward as you commit
- HEAD is where you are “pointing” (directly or via a branch)

Operator implication:

- “lost” commits are often not lost; they’re just not referenced
- reflog can often recover “oops” moments

## Three Safety Levels of Change

### Safe (Low Risk)

- new commits on a branch
- merge commits
- revert commits

These preserve history and reduce surprise in shared repos.

### Medium Risk (Coordination Required)

- rebase of a branch that other people may be using
- force-push to a shared branch (even if allowed)

### High Risk (Avoid in Shared History)

- rewriting public history without explicit coordination
- deleting branches/tags without replacement plan

## Commit Hygiene as an Operational Tool

Commit messages and structure are not aesthetics; they support:

- incident forensics (“what changed?”)
- targeted rollback (revert a specific change)
- release notes and changelogs

Minimum good commit message:

- intent + scope
- what changed, not how you felt

## The “Two Truths” of Git for DevOps

1) You must be able to move fast with changes.
2) You must be able to reconstruct what happened after a failure.

That is why you learn both:

- feature workflows (branches, PRs)
- recovery workflows (reflog, bisect, revert, cherry-pick)
