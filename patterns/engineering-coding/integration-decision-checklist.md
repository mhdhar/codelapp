---
title: "Integration Decision Checklist"
slug: "integration-decision-checklist"
format: "goal"
category: "engineering-coding"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "Before adding any new dependency or service, check for an existing alternative and the real cost first."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're about to install a new library or wire up a new third-party service, and want to make sure it's actually needed before it becomes one more thing to maintain, key-rotate, and keep updated. Use it before adding the dependency, not after it's already in package.json.

## The pattern
```text
I'm considering a new dependency. Run it through a decision checklist before I install anything.

1. Check what's already in the project (package.json / existing services) — is there something already installed that covers 80%+ of this need without adding a new dependency?
2. If it's a well-known service category (auth, payments, email, file storage, error monitoring, etc.), name the 1-2 most common choices and note if any has first-class agent/CLI tooling that would make it faster to integrate and maintain than a lesser-known alternative.
3. State the real ongoing cost of adding this: a new API key to manage, a new thing that can break/rate-limit/go down, a new item in the dependency update cycle. Be concrete, not generic.
4. Give a clear go/no-go recommendation, and if go, the exact install command.
5. If no-go, name the lighter alternative (an existing dependency, a small hand-rolled function, or "you don't need this yet").

Start by asking me, in one message, which library or service I'm considering and what problem it solves. Wait for my answer before running the checklist.
```

## Real example output
```
> Library or service: lodash
> Problem it solves: need a deep object comparison for a diffing feature.

1. Checked package.json — no existing deep-equal utility. Node's built-in util.isDeepStrictEqual exists but isn't imported anywhere yet.

2. Not really a "well-known service category" case — this is a narrow utility need, not auth/payments/etc.

3. Real cost of lodash: 70KB+ if not tree-shaken correctly (this project doesn't have tree-shaking configured in its current build), one more dependency to keep patched, and you'd likely only use 1 of its ~300 functions.

4. No-go on lodash for this. Use Node's built-in util.isDeepStrictEqual instead — zero new dependency, zero new attack surface, does exactly what's needed.

5. Alternative: `import { isDeepStrictEqual } from 'node:util'` — already available, no install step.
```

## Why it works
Every dependency added is a standing cost, updates, security patches, a key to manage if it's a service, that outlives the five minutes it saved during implementation. Checking for an existing alternative first turns "just install it" into a decision you'd stand behind six months later.
