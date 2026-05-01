---
title: Common Mistakes (Wrong vs Right)
tags:
  - mistakes
  - security
module: "10"
---

# Common Mistakes — Module 10 Security

## 1) Wrong: Log Secrets for Debugging

Wrong pattern:

- printing tokens/keys to logs to “see what’s happening”

Right pattern:

- verify presence by behavior and scoped checks without printing values

## 2) Wrong: Assume “Temporary Token” Is Safe

Wrong pattern:

- treat short-lived tokens as non-secrets

Right pattern:

- treat all credentials as secrets; rotate if leaked

## 3) Wrong: PR Jobs Access Production Secrets

Wrong pattern:

- secrets available to untrusted PR contexts

Right pattern:

- strict environment separation; approvals for prod deploy

## 4) Wrong: Use :latest Everywhere

Wrong pattern:

- floating base images and dependencies

Right pattern:

- pin versions and record inputs; promote by digest where possible

## 5) Wrong: Security as a One-Time Checklist

Wrong pattern:

- scan once, ignore later

Right pattern:

- continuous guardrails + incident-ready runbooks
