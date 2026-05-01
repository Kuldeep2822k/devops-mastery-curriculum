---
title: Common Mistakes (Wrong vs Right)
tags:
  - mistakes
  - networking
module: "15"
---

# Common Mistakes — Module 15 Networking Protocols

## 1) Wrong: “It’s the Network” Without Classification

Wrong pattern:

- blame network before checking listener vs DNS vs TLS

Right pattern:

- classify refused vs timeout vs TLS failure first

## 2) Wrong: Disable TLS Verification

Wrong pattern:

- `-k` / insecure defaults as fix

Right pattern:

- fix time drift, cert chain, hostname/SNI issues

## 3) Wrong: Ignore DNS Caching

Wrong pattern:

- expect DNS change to apply instantly everywhere

Right pattern:

- account for TTL and multiple resolvers

## 4) Wrong: Retry Forever

Wrong pattern:

- unbounded retries amplify outages

Right pattern:

- bounded retries with backoff and stop conditions

## 5) Wrong: Skip Evidence Capture

Wrong pattern:

- restart and lose logs

Right pattern:

- capture evidence, then change one variable at a time
