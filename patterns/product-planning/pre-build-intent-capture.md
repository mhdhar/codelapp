---
title: "Capture Intent Before You Scaffold Anything"
slug: "pre-build-intent-capture"
format: "prompt"
category: "product-planning"
tools: ["universal"]
difficulty: "beginner"
est_time: "5 min"
models: ["any"]
summary: "Answer four questions before you scaffold anything, so the build starts pointed at the right target."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're about to type a scaffold command or open a fresh repo for a new project, but you haven't actually decided what you're building. Run this first, before a single file exists, not after you've already committed to a stack.

## The pattern
```text
Before I scaffold anything, force me to answer these four questions about the project idea I paste below the line:

1. One-line description: what does this do, in one sentence a stranger would understand.
2. Target user: who specifically uses this first (not "everyone").
3. API surface: will this project expose an API or MCP server for other tools/agents to call, now or in the next 2 releases? Yes/no and why.
4. SaaS shape: is this a conventional SaaS (auth + payments + admin dashboard + landing page), or does it need a non-standard architecture? If conventional, name the preset/boilerplate that already covers it instead of building from scratch.

Answer all four in under 150 words total. If you don't have enough information to answer any of them confidently, say which one and ask me directly, don't guess.

My project idea:
```

## Real example output
"1. One-line description: A tool that scans receipt photos and auto-categorizes expenses into a spreadsheet-ready format.
2. Target user: Freelancers doing their own quarterly taxes, not accountants or small-business bookkeepers.
3. API surface: No, not in the next 2 releases. Revisit if a bookkeeping SaaS asks to integrate.
4. SaaS shape: Conventional SaaS (auth + Stripe + dashboard). Use an Open SaaS-style preset instead of scaffolding from scratch — saves roughly 2-3 weeks of boilerplate work on auth and billing."

## Why it works
Answering these four up front kills two failure modes at once: architecture chosen by default instead of by comparison, and an API surface decided by accident three months in after auth and versioning assumptions are already baked in. Naming the SaaS-shape question specifically forces preset-vs-custom to be a real choice instead of a habit.
