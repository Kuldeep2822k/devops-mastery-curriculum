---
title: Common Mistakes (Wrong vs Right)
tags:
  - mistakes
  - programming
module: "13"
---

# Common Mistakes — Module 13 Programming for DevOps

## 1) Wrong: No Timeouts

Wrong pattern:

- scripts hang forever on network calls

Right pattern:

- set timeouts and fail with clear error

## 2) Wrong: Unbounded Retries

Wrong pattern:

- infinite retry loops

Right pattern:

- bounded retries with backoff and stop conditions

## 3) Wrong: Unsafe Defaults

Wrong pattern:

- destructive by default

Right pattern:

- dry-run default and explicit --apply

## 4) Wrong: Grep JSON

Wrong pattern:

- parsing JSON with regex pipelines

Right pattern:

- use a JSON parser and validate schema

## 5) Wrong: Log Secrets

Wrong pattern:

- print tokens or payloads containing credentials

Right pattern:

- log evidence without secret values
