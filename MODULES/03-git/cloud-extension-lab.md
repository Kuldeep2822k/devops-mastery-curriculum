---
title: Cloud Extension Lab (Optional)
tags:
  - cloud
  - optional
  - git
module: "03"
---

# Cloud Extension Lab (Optional) — Git and GitHub

Core learning is local-first. This extension maps collaboration controls to a hosted Git platform.

## Goal

Create a hosted repo and practice:

- protected main branch behavior (conceptually; exact UI differs by provider)
- pull request workflow with required checks
- a release tag and a rollback by revert
- incident-style “regression hunt” workflow using PR and release metadata

## Cost Control

This lab should be free-tier for most users. Still:

- delete test repos if you do not need them
- avoid uploading large artifacts

## Steps (High Level)

1. Create a repository `mod03-git-cloud`.
2. Configure protections:
   - require PR review before merge
   - require status checks (use a minimal CI later in Module 04)
3. Open a PR that introduces a regression (use a simple script and test).
4. Merge it and create a release tag.
5. Rollback by revert PR (or revert commit) and tag a new release.

## Verification Signals

- PR requires review before merge
- main branch shows a clear history of merge and revert
- tags map releases to commits

## Cleanup

- delete the repo or archive it if not needed
- remove any tokens/keys used during setup
