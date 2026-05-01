---
title: Review Questions
tags:
  - review
  - linux
module: "02"
---

# Review Questions — Module 02 Linux

Answer from memory first.

1. What are the first 5 commands you run when a service is “down” on a Linux host? What does each tell you?
2. Explain SIGTERM vs SIGKILL. When is SIGKILL justified?
3. What is a zombie process? How do you detect it and what is the real fix?
4. Explain what `load average` means and why it can be high even when CPU% looks low.
5. What evidence do you capture before restarting a failing service?
6. Explain directory execute permission and why “file is readable” is not enough.
7. Why is “chmod 777” a bad default? What are safer alternatives?
8. What is the difference between “connection refused” and “timeout”? How do you diagnose each?
9. In systemd, what does `Restart=on-failure` mean? When can `Restart=always` make things worse?
10. How do you view the last 15 minutes of logs for a systemd service? How do you follow logs?
11. What are common causes of crash loops for systemd services?
12. How do you detect disk pressure and find the biggest consumers quickly?
13. Describe a safe plan to raise file descriptor limits for a service, including verification.
14. Write a minimal runbook outline for “service not listening on port”.
