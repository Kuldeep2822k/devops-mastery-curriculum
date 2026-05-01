---
title: Testing and Packaging (Make Targets, Minimal Tests)
tags:
  - programming
  - testing
module: "13"
---

# Testing and Packaging (Make Targets, Minimal Tests)

## Why Tests Matter for Ops Tools

Ops tools break production when:

- edge cases are unhandled
- assumptions change (API responses, file formats)

Even minimal tests reduce risk:

- unit tests for parsing/formatting
- integration tests against a local fake server

## Make Targets as a Contract

Recommended:

- `make lint`
- `make test`
- `make run`
- `make package`

CI should run the same targets.

## Packaging Guidance (Practical)

You can ship a tool as:

- a single script with pinned dependencies
- a container image
- a Python package

Operator requirement:

- reproducible execution and clear versioning.

## Anti-Patterns

- running unversioned scripts from random gists
- no tests and no dry-run
- no dependency pinning
