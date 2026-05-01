---
title: '02-git-ci-demo: Interview Explanation'
tags:
  - project
  - interview
---

# 02-git-ci-demo — Interview Explanation

## Problem

Build a small repo with a CI-like local pipeline that produces artifacts, caches safely, and enforces baseline gates (format/test/build).

## Approach

- Local-first build so the work is reproducible
- Evidence-driven verification and cleanup
- Failure drills to force operational thinking

## Tradeoffs

- What was simplified for local-first constraints
- What would change in production (scale, IAM, managed services)

## Failures + Fixes

- Describe the top 3 break/fix drills and what you learned

## What I Would Do Next

- Hardening, observability improvements, and guardrails
