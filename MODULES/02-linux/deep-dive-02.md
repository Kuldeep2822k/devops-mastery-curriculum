---
title: "Deep Dive 02: systemd Unit Design and Operational Tradeoffs"
tags:
  - linux
  - deep-dive
  - systemd
module: "02"
---

# Deep Dive 02 — systemd Unit Design and Operational Tradeoffs

## Unit Design Is Part of Reliability

A service unit is a contract:

- how the service starts
- which user it runs as
- how it is restarted
- where logs go
- what environment it sees

Bad unit design increases MTTR and creates crash loops.

## Restart Policies

Common choices:

- `Restart=on-failure`: restart on non-zero exits
- `Restart=always`: can hide repeated failure patterns and cause flapping
- `Restart=no`: forces explicit operator action, but can increase downtime

Tradeoff thinking:

- If a service is critical, on-failure restarts can reduce downtime.
- If failures indicate bad config or migrations, automatic restarts can worsen impact.

## Start Limits and Flap Control

systemd can rate-limit restarts:

- prevents infinite loops
- makes failures visible

Operators should understand why a service stops restarting:

- it is often a protective mechanism, not “systemd broken”

## Environment Variables and Secrets

Environment variables are convenient, but risky:

- they leak into process inspection tools
- they can be captured in diagnostics

Safer patterns:

- config files with strict permissions
- systemd drop-ins for non-secret toggles

Never put secrets in unit files stored in source control.

## WorkingDirectory and Relative Paths

Common failure:

- binary expects relative file paths
- WorkingDirectory not set, service fails

Fix:

- use absolute paths in services
- set WorkingDirectory explicitly if required

## File Descriptor Limits

Production services often need increased limits:

- many connections
- lots of open files

But increasing limits blindly can hide leaks.

Tradeoff:

- raise limits with evidence (connection counts, EMFILE errors)
- add observability to track fd usage

## Safe Changes

When editing a unit:

- change one thing at a time
- reload systemd daemon
- verify status and logs
- have a rollback plan (previous unit file)
