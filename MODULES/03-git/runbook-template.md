---
title: Runbook Template (Module 03)
tags:
  - runbook
  - template
  - git
module: "03"
---

# Runbook Template — Module 03 Git

## Title

<Repo> — <Incident (e.g., regression after merge)>

## Impact

- What is broken in production?
- Which version/tag is deployed?
- Severity and scope:

## Safety and Preconditions

- Stop risky actions:
  - no force-push to shared branches
  - no history rewrite without coordination
- Preserve evidence:
  - capture deployed version and commit SHA
  - capture failing signal (test, smoke check, logs)

## Identify Change Window

- Current deployed tag:
- Previous known-good tag:

Commands:

```bash
git fetch --all --prune
git log --oneline --decorate --graph --all -n 30
git tag --list | tail -n 20
git show <tag>
```

## Diagnose Regression

### Define Good/Bad Signal

- signal:
- how to run it:

### Bisect Procedure

```bash
git bisect start
git bisect bad
git bisect good <known_good_commit>
```

At each step:

- run signal
- mark:

```bash
git bisect good
git bisect bad
```

Capture result:

```bash
git bisect log
git show --stat
```

## Containment / Rollback

Preferred: revert the causal commit (or revert the merge commit).

Commands:

```bash
git bisect reset
git revert <commit>
```

If reverting a merge commit:

```bash
git revert -m 1 <merge_commit>
```

## Verification

- run the same signal used for bisect
- ensure build/tests pass
- confirm tag/release metadata

## Post-Incident Prevention

- add/strengthen deterministic tests
- tighten CI checks and branch protections
- document rollback steps and triggers
