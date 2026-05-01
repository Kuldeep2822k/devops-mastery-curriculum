---
title: Common Mistakes (Wrong vs Right)
tags:
  - mistakes
  - linux
module: "02"
---

# Common Mistakes — Module 02 Linux

## 1) Wrong: Restart as First Move

Wrong pattern:

- `systemctl restart` without reading status/logs

Right pattern:

- capture status + last logs + sockets
- then choose containment (rollback/restart) with a reason

## 2) Wrong: “It’s a Network Issue”

Wrong pattern:

- blame network before verifying local listener

Right pattern:

- `ss -tulpn` and local `curl`/`nc` first
- then expand outward (DNS/firewall/upstream)

## 3) Wrong: chmod/chown Randomly

Wrong pattern:

- `chmod 777` or `chown -R` on broad directories

Right pattern:

- identify service user
- adjust only the required paths with least privilege

## 4) Wrong: Ignore Disk Until It Breaks

Wrong pattern:

- no disk checks until services crash

Right pattern:

- routinely check `df -h` and biggest directories
- bound log growth and container image growth

## 5) Wrong: Treat “Running” as “Healthy”

Wrong pattern:

- process exists, so service is fine

Right pattern:

- verify listener, endpoints, and dependency health
