---
title: '01-linux-admin-lab: Interview Explanation'
tags:
  - project
  - interview
---

# 01-linux-admin-lab — Interview Explanation

## Problem

Debug and recover a Linux host under realistic failure modes (service down, disk full, DNS/network misconfig), producing a runbook and postmortem-quality evidence.

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
