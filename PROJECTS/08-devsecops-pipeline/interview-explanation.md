---
title: '08-devsecops-pipeline: Interview Explanation'
tags:
  - project
  - interview
---

# 08-devsecops-pipeline — Interview Explanation

## Problem

Implement baseline security gates: no secrets, pinned dependencies, and an SBOM/manifest artifact stored with the build.

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
