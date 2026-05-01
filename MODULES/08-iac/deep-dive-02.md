---
title: "Deep Dive 02: Drift as an Organizational Failure"
tags:
  - terraform
  - deep-dive
  - drift
module: "08"
---

# Deep Dive 02 — Drift as an Organizational Failure

## The Drift Loop

1) someone changes infra manually to “fix it fast”  
2) Terraform plan later shows surprising diffs  
3) apply becomes risky and confusing  
4) team avoids Terraform and does more manual changes  

This is how IaC adoption fails.

## Staff-Level Fix: Make the Right Path the Easy Path

- provide fast, safe Terraform workflows for urgent changes
- keep diffs small and applies quick
- document runbooks for common changes (scale, DNS, firewall, secrets rotation)
- implement drift detection and alerts

## Drift Policies (Examples)

- strict: manual changes forbidden; drift triggers incident/process review
- controlled: manual changes allowed only with immediate IaC follow-up PR

## What to Capture During Incidents

- exact manual change (who/what/when/why)
- temporary nature (expires_on)
- follow-up task to reconcile IaC

Treat it like any other reliability incident: root cause and prevention.
