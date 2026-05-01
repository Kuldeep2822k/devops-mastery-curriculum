---
title: 'Troubleshooting Lab'
tags:
  - troubleshooting-lab
  - identity
  - oauth
  - oidc
  - rbac
module: "22"
---

# Troubleshooting Lab — Module 22 Identity and Access (OIDC/OAuth/RBAC)

Complete at least 12 scenarios with a time-boxed incident log and evidence.

- Requests time out only under load; low traffic looks fine.
- A deploy succeeds but a subset of users get 401/403.
- Latency jumps after enabling a new sidecar/proxy layer.
- A background worker falls behind and creates backlog.
- A dependency is healthy but your service is saturating.
- A certificate rotates and only some clients fail.
- A queue delivers duplicates and downstream is not idempotent.
- A rollback restores service but leaves data/schema inconsistent.
- Alerts fire but dashboards look normal (alert quality issue).
- A regional outage causes cascading retries across regions.
- A secret expires and the error message is misleading.
- A “fix” increases error rate because of hidden coupling.
