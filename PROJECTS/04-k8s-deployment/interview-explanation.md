---
title: '04-k8s-deployment: Interview Explanation'
tags:
  - project
  - interview
---

# 04-k8s-deployment — Interview Explanation

## Problem

Deploy a service to local Kubernetes with safe rollouts, readiness/liveness probes, service routing, and rollback drills.

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
