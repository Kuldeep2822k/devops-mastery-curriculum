---
title: Start Here (Absolute Beginners)
tags:
  - start-here
  - beginners
  - onboarding
---

# Start Here 👋

**New to all of this? Read this page first — before the README, before Setup, before Module 01.**

This guide is deep and ambitious ("Zero to Staff"). That's a strength once you're moving, but it also means the later material is genuinely advanced. This page is your gentle on-ramp: what DevOps actually is, who this guide is for, what you need to know before you start, and the exact order to follow so you don't get lost.

## What is DevOps? (in plain English)

Software has two classic problems:

1. **Building it** — writing the code.
2. **Running it** — getting that code onto real computers ("servers"), keeping it up, noticing when it breaks, and fixing it fast.

For a long time, one team wrote the code ("Dev") and a different team ran it ("Ops"), and they argued a lot. **DevOps** is the practice of merging those worlds: the same people (and the same automation) build, ship, run, and fix software — using code and tooling to make that loop fast, safe, and repeatable.

Concretely, a DevOps / SRE / Platform engineer spends their day on things like:

- **Delivery** — turning code into a running program automatically (build → test → deploy), instead of copying files by hand.
- **Operations** — keeping services healthy: is it up? is it fast? did the last change break something?
- **Observability** — collecting signals (logs, metrics) so you can *see* what a system is doing.
- **Incident response** — when something breaks at 2 a.m., diagnosing it calmly and restoring service.
- **Reliability & safety** — making changes that can be undone, and preventing the same fire twice.

> **The mental model for the whole field:** `build → deploy → run → observe → respond → improve`, on repeat. Almost every module in this guide is a deeper look at one part of that loop.

You don't need to memorize any of that yet. You'll build it up module by module.

## Who this guide is for (honest prerequisites)

The title says "Zero to Staff." Be aware of what the **"zero"** means: it's *zero staff-level operations experience* — **not** zero computer skills. To have a good time here, you should be comfortable with a few basics first:

- Using a **terminal / command line** (opening it, typing commands, moving between folders).
- **Editing a text file** and saving it.
- The rough idea of what **Git** and **GitHub** are (even if you've never committed).
- Being on **Linux, macOS, or Windows with WSL2** (see the setup notes below).

**Never touched a terminal or Git? That's completely fine — start with the primer:**
👉 [SETUP/00-prerequisites-primer.md](SETUP/00-prerequisites-primer.md)

It takes you from "what is a terminal" to "I made my first commit," and it's the missing first step for a true beginner.

> **You do not need:** a cloud account, a powerful computer, prior programming experience, or money. The core path runs locally and free.

## The gentle path (do it in this order)

Ignore the "365-day mastery" and the 25-module map for now — that's the *whole mountain*. Here is your **first trail**:

1. **Read this page** (you're doing it). ✅
2. **Do the primer** if you're new to the terminal/Git: [SETUP/00-prerequisites-primer.md](SETUP/00-prerequisites-primer.md).
3. **Install just enough tooling.** For your very first lab you only need **Python 3** and **curl** (and **Git** to clone this repo) — not the whole toolbox. See [SETUP/00-overview.md](SETUP/00-overview.md), which now tells you what each module actually requires.
4. **Do [Module 01 — Foundations](MODULES/01-foundations/00-overview.md)**, but treat the *concept* pages as a first read, not a test. Then do **lab-01**, where you'll run a real service and watch it respond. That first working lab is your first win — get to it early.
5. **Keep the glossary open in another tab.** Whenever you hit a term you don't know (SLO, idempotent, canary…), look it up: [APPENDICES/glossary.md](APPENDICES/glossary.md).
6. **Work through Modules 01–12** (the "Core") at a calm pace. That alone makes you a capable junior operator.

Everything after Module 12, plus the **deep-dive** pages inside each module, is **advanced**. It's there for when you're ready — not for week one.

## How hard is each part? (so you can pace yourself)

| Part | Level | When to tackle it |
| --- | --- | --- |
| `SETUP/00-prerequisites-primer.md` | 🟢 Beginner | First, if terminal/Git is new to you |
| Module **01–12** concept pages + labs | 🟢🟡 Beginner → Intermediate | Your main path |
| The `deep-dive-*.md` pages in any module | 🔴 Advanced | Skip on first pass; return later |
| Modules **13–25** | 🔴 Advanced / Staff | After you're comfortable with the Core |
| Capstones & "Brownfield Rescue" | 🔴 Staff | Much later — these simulate real senior work |

> **Callout — you are not failing if the deep dives feel hard.** They're written for people with a year+ of experience. Read the `01–04` concept pages and do the labs; that's the beginner path. Come back to the deep dives when the basics feel easy.

## If you get stuck

- **A term makes no sense** → [Glossary](APPENDICES/glossary.md).
- **A command isn't found / setup broke** → each setup doc has a Troubleshooting section; also see [SETUP/01-workstation-baseline.md](SETUP/01-workstation-baseline.md).
- **You're on Windows** → follow the **WSL2** path in [SETUP/01-workstation-baseline.md](SETUP/01-workstation-baseline.md#windows-users-use-wsl2).
- **You feel overwhelmed by the size** → that's normal. Do only steps 1–6 above. Ignore the rest until you finish the Core.

## Where to go next

- The full picture and philosophy: [README.md](README.md)
- The learning method (how to actually retain this): [00-HOW-TO-USE/00-how-to-use.md](00-HOW-TO-USE/00-how-to-use.md)
- The module map: [ROADMAP.md](ROADMAP.md)

Welcome aboard. Take it one small win at a time. 🚀
