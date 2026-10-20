---
name: using-bugbash
description: Use when the user wants to find bugs, stress-test, QA, exploratory-test, or verify behavior by exercising the running system—not for routine implementation, refactors, or fix-focused debugging unless they ask to switch modes after findings exist.
---

# Using Bugbash

Bugbash is **not** a general development methodology. It is a **narrow contract**: spend effort **proving** whether the software misbehaves, under **clear boundaries**, with **evidence** you could hand to another engineer.

## Philosophy (vs generic “agent frameworks”)

- **Outcomes over process:** The win is a **reproducible finding** or a **confident “clean for this mission”**—not ticking every ritual step.
- **Execution first:** Prefer **running** the product; code is for **triage after** you have signals.
- **Honest uncertainty:** Separate **confirmed** bugs from **flakes**, **heuristics**, and **test gaps**—do not blur them.
- **No stealth fixes:** Do not slip patches in under the guise of “testing.” Fixes are a **different** user request.

## When this applies

Bugbash fits when the user (or parent agent) wants things like: bug bash, QA pass, stress test, break it, edge cases, negative testing, soak, concurrency worries, release confidence.

**Usually a mismatch:** pure code review without running anything, architecture debates, “why is this slow” deep profiling, or step-by-step debugging where the user already has a stack trace and wants a fix.

## What to do

1. If the session needs boundaries (environment, timebox, destructive OK), follow **Mission brief** in **`bugbash-core`** first—keep it short.
2. Load **`bugbash-core`** with the **Skill** tool and run it at the **intensity** the user asked for (`quick`, `standard`, or `deep`). Default to **standard** if unspecified.
3. For parallel work, split by **surface**, not by “more generic testers”: e.g. **`bugbash-api`** and **`bugbash-ui`** together, then merge into one tracker.

## User overrides

`CLAUDE.md`, `AGENTS.md`, and direct instructions win. If the user forbids network, shell, or destructive actions, obey and say what could not be exercised.

## Platform note

In Claude Code, load skills with the **Skill** tool rather than reading skill files ad hoc.
