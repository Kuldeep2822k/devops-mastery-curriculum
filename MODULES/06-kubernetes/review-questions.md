---
title: Review Questions
tags:
  - review
  - kubernetes
module: "06"
---

# Review Questions — Module 06 Kubernetes

Answer from memory first.

1. Explain the difference between Deployment, ReplicaSet, and Pod. Which do you “fix” during incidents?
2. What are labels and selectors? Give a concrete example of a selector-caused outage.
3. Explain readiness vs liveness vs startup probes. When can a liveness probe make an incident worse?
4. What does a Pending pod mean? List at least five root causes.
5. What evidence indicates ImagePullBackOff and what are common root causes?
6. What does OOMKilled mean in Kubernetes? How do you distinguish limit too low vs memory leak?
7. Walk through the debugging playbook: events → describe → logs → exec → network checks.
8. Service returns 503. What are your first 8 commands and why?
9. Ingress misroutes traffic. What objects do you inspect and what signals do you look for?
10. DNS failures: how do you debug resolution from inside the cluster?
11. RBAC denies: what is your first hypothesis and how do you prove it?
12. What tradeoffs belong in an ADR about resource limits and probe strategy?
