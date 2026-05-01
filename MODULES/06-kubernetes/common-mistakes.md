---
title: Common Mistakes (Wrong vs Right)
tags:
  - mistakes
  - kubernetes
module: "06"
---

# Common Mistakes — Module 06 Kubernetes

## 1) Wrong: Delete Pods Until It Works

Wrong pattern:

- delete pods without capturing logs/events

Right pattern:

- capture events and logs first
- fix controller config (Deployment), not individual pods

## 2) Wrong: Ignore Endpoints

Wrong pattern:

- assume Service routes because pods are running

Right pattern:

- always check endpoints for Service 503 incidents

## 3) Wrong: Probes as Copy/Paste

Wrong pattern:

- liveness/readiness copied from another service without considering startup time and dependencies

Right pattern:

- define probe semantics and tune initial delays and timeouts with evidence

## 4) Wrong: Limits Without Requests (or Evidence)

Wrong pattern:

- limits set arbitrarily; requests omitted

Right pattern:

- requests set for scheduling; limits set for safety; tune from observed usage

## 5) Wrong: DNS Panic

Wrong pattern:

- “DNS is broken” without checking service name, endpoints, and cluster DNS pods

Right pattern:

- verify name, endpoints, and DNS resolution from inside a pod
