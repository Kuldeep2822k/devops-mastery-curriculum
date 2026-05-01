---
title: Branching and Merge Strategies
tags:
  - git
  - branching
  - merges
module: "03"
---

# Branching and Merge Strategies

## A Branching Strategy Is a Risk Strategy

You choose branching and merge behaviors to control:

- how quickly changes ship
- how easy rollback and forensics are
- how much coordination is required

## Core Options

### 1) Merge Commits

Pros:

- preserves the shape of development
- can make rollback by PR easier (revert the merge commit)

Cons:

- noisier history

### 2) Squash Merge

Pros:

- linear history
- one “unit” per PR

Cons:

- loses granular commit structure
- harder to bisect inside a PR without extra work

### 3) Rebase + Fast-Forward

Pros:

- clean linear history
- bisect-friendly when commits are well-formed

Cons:

- requires discipline to avoid rewriting shared history
- conflicts can be repeated across rebases

## Conflict Resolution as a Required Skill

Conflicts are inevitable:

- parallel changes
- long-lived branches
- hotfixes during incident response

Operator habit:

- resolve conflicts by understanding intent, not by “taking ours”
- verify after merge (tests, build, smoke checks)

## Hotfix Strategy (Production Reality)

When production is broken:

- you need a path to ship a hotfix quickly
- you need a path to undo it quickly if wrong

Common safe approach:

- create a hotfix branch from the release tag (or main if you have no tags yet)
- make minimal change
- release via the same pipeline
- tag the release

## Anti-Patterns

- long-lived branches that drift for weeks
- force-pushing shared branches as a default
- “merge and pray” without verification signals
- mixing refactors and behavior changes in one PR when risk is high
