---
title: 'Common Mistakes'
tags:
  - common-mistakes
  - datastores
  - postgres
  - redis
module: "20"
---

# Common Mistakes — Module 20 Datastores for DevOps (Postgres/Redis)

- Changing multiple variables at once during an incident.
- Using unbounded retries/timeouts and creating retry storms.
- Disabling verification (TLS/auth) as a “fix” without a rollback plan.
- Not capturing a minimal incident log (symptoms, timestamps, scope).
