---
title: Release Gates and Progressive Delivery
tags:
  - delivery
  - progressive-delivery
  - release-gates
module: "07"
---

# Release Gates and Progressive Delivery

## Gates: What They Are

Gates are decision points that control risk.

Examples:

- “all checks passed” gate (CI)
- “manual approval” gate (high-risk environments)
- “smoke checks pass” gate (post-deploy)
- “SLO not violated” gate (canary progression)

## Progressive Delivery Without Fancy Tools

You can do progressive delivery with basic primitives:

- canary Deployments with smaller replicas
- traffic splitting (ingress controller features) or coarse selection (service switch)
- observation windows and explicit stop/rollback triggers

Tools can help (later modules/projects), but the operator skill is:

- defining the signals and making the go/no-go call.

## Environment Separation and Approvals

Use approvals when:

- changes impact billing/security/compliance
- rollback is risky
- deploy path can access sensitive secrets

Approval anti-pattern:

- approvals used as “I didn’t look but I clicked approve”

Approval good pattern:

- approval includes a checklist:
  - what changed
  - what to watch
  - rollback plan
  - who is on-call

## Release Metadata (Minimum)

For every production deploy, record:

- artifact identity (digest/tag)
- source revision (commit SHA)
- change summary
- who approved (if applicable)

This becomes critical during incident forensics.
