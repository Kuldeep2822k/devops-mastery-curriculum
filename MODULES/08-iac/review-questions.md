---
title: Review Questions
tags:
  - review
  - terraform
module: "08"
---

# Review Questions — Module 08 IaC (Terraform)

Answer from memory first.

1. What does Terraform state contain and why is it sensitive?
2. What does `terraform plan` guarantee and what does it not guarantee?
3. Why is concurrent apply dangerous? What does locking prevent?
4. Define drift. Why is drift usually a process failure rather than a tooling failure?
5. When would you use `terraform state mv`? Give a safe refactor example.
6. Why is `terraform state rm` dangerous? When might it be justified?
7. Describe a safe workflow for production changes (plan, review, apply, verify).
8. Compare workspaces vs separate directories/states for environments.
9. What does `create_before_destroy` do and what risks can it introduce?
10. Why do unpinned providers cause outages?
11. What belongs in an ADR about your Terraform layout and backend choice?
12. What are the first 8 commands you run when Terraform apply fails unexpectedly?
