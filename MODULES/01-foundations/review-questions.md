---
title: Review Questions
tags:
  - review
  - foundations
module: "01"
---

# Review Questions — Module 01 Foundations

Answer without looking at notes first. Then check your answers and tighten them.

## Systems Thinking

1. Draw a dependency graph for a simple HTTP service. What are the minimum nodes you include and why?
2. What are “invariants” for a service? Give three and explain how you verify each.
3. What is a failure domain? Give two examples in local labs and two in cloud systems.
4. Why do incidents often have multi-layer root causes? Provide an example.

## Environments and Change

5. What does “environment separation” mean beyond “different URLs”?
6. What makes a rollback safe? Describe at least three requirements.
7. When would you prefer forward-fix over rollback during a high-severity incident?

## Reliability and Signals

8. Define RED metrics. Which are easiest to fake, and which are hardest to fake?
9. What is an SLO? What is an SLI? Give an example of each for `/healthz`.
10. Explain error budgets in plain language. How do they influence change velocity?

## Operational Writing

11. What must a runbook include to be useful under pressure?
12. What information belongs in an ADR that is commonly missing?

## Practice Prompts

13. You see 200 OK but latency doubled. What are your first 5 diagnosis steps?
14. A health endpoint returns 500 intermittently. What hypotheses do you form and how do you test them?
