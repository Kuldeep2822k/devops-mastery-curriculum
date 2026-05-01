---
title: Cloud Extension Lab (Optional)
tags:
  - cloud
  - optional
  - networking
module: "15"
---

# Cloud Extension Lab (Optional) — Real Network Debugging

Core learning is local-first. This extension maps the protocol playbook to real environments:

- VPC/VNet routing
- security groups/firewalls
- managed DNS and split-horizon
- TLS at load balancers and ingress

## Goal

Practice:

- reproducing a timeout vs refused vs TLS failure across networks
- capturing evidence (flow logs, LB logs, DNS query logs) where available
- applying minimal fixes with rollback

## Cost Control (Mandatory)

- set budgets and alerts before provisioning
- delete resources after verification

Read: [Lab Safety and Cost Control](../../00-HOW-TO-USE/03-lab-safety-cost-control.md)

## Verification Signals

- you can isolate failure domain in under 10 minutes with a repeatable playbook
