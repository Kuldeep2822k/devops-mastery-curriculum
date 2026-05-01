---
title: Review Questions
tags:
  - review
  - ci
module: "04"
---

# Review Questions — Module 04 CI

Answer from memory first.

1. Draw a pipeline with stages, artifacts, caching, and gates. What are the failure modes of each stage?
2. What is an artifact contract? Give an example with `dist/` and `manifest.txt`.
3. How do you design cache keys to avoid lockfile drift issues?
4. When does fail-fast help, and when does it hurt?
5. Why is “rebuild in CD” an anti-pattern for provenance and rollback?
6. Name 5 ways secrets leak in CI and how to prevent each.
7. Explain environment separation: what should PR validation never be able to do?
8. What causes flaky tests? Give 5 causes and 5 mitigations.
9. How do timeouts create both false failures and hidden slow failures?
10. What is a quarantine policy? What is the risk of quarantining tests?
11. What metrics define CI health? Propose a minimal CI SLO.
12. A pipeline is slow. How do you determine whether caching, parallelism, or test splitting is the best lever?
