---
title: Cloud Extension Lab (Optional)
tags:
  - cloud
  - optional
  - linux
module: "02"
---

# Cloud Extension Lab (Optional) — Linux Operations

Core learning is local-first. This extension maps Linux operational practice to cloud VMs.

## Goal

On a cloud VM, practice:

- service installation and systemd unit management
- journald log inspection
- network triage (security groups/firewall vs local listener)
- resource pressure diagnosis (CPU/mem/disk)

## Cost Control (Mandatory)

Before provisioning:

- set budgets/alerts
- tag all resources with owner, purpose, expires_on, module
- choose smallest instance types
- delete resources immediately after verification

Read: [Lab Safety and Cost Control](../../00-HOW-TO-USE/03-lab-safety-cost-control.md)

## Option A: AWS

High-level mapping:

- EC2 instance (small)
- systemd unit for a demo service
- security group allowing only SSH and one service port from your IP

Drills:

- break ExecStart path and recover
- block port in security group and diagnose “timeout vs refused”

## Option B: Azure

High-level mapping:

- VM + Network Security Group rules
- same drills and diagnosis approach

## Option C: GCP

High-level mapping:

- Compute Engine VM + firewall rules
- same drills and diagnosis approach

## Verification Signals

- service active/running in systemd
- expected logs in journald
- listener visible in `ss -tulpn`
- remote connectivity behaves as expected under firewall changes

## Cleanup

- delete VM, disks, public IPs, firewall rules
- verify no remaining resources in the account/project/subscription
