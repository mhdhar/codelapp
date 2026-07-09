---
title: "Complexity Tier Classifier"
slug: "complexity-tier-classifier"
format: "goal"
category: "engineering-coding"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "beginner"
est_time: "10 min"
models: ["any"]
summary: "Classify a new build into a size tier before scaffolding, so you don't over- or under-build it."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
Right before scaffolding any new project or major feature, when you haven't consciously decided how much infrastructure it actually deserves. Use it to right-size the build before the first line of boilerplate gets written.

## The pattern
```text
Classify what I'm about to build into a complexity tier before we scaffold anything. What I'm building, who it's for, and how long it lives are filled in at the end of this message.

Tiers:
- NANO: single file or single page, no backend, no auth, disposable.
- MICRO: one feature, one screen, optional lightweight backend, short-lived or low-stakes.
- STANDARD: multiple features, real auth, a real database, meant to be maintained.
- PRO: production product with multiple environments, monitoring, and real users depending on uptime.
- ENTERPRISE: multi-tenant and/or compliance-heavy, custom infra required.

1. Pick the tier that fits, and say which specific inputs (users, lifespan, feature count) drove the choice.
2. Warn me explicitly if what I described sounds like it's aimed at one tier but the actual scaffolding request implies a higher one (e.g. asking for a NANO script but also asking for user accounts and a database).
3. Recommend the minimum stack that fits the chosen tier — do not recommend infrastructure the tier doesn't need.
4. State what would bump it up a tier later, so I know the upgrade trigger in advance.

What I'm building:
Expected users:
Lifespan:
```

## Real example output
```
What I'm building: a script that pulls weekly analytics numbers and posts them to Slack.
Expected users: just me and 2 teammates who read the Slack message.
Lifespan: ongoing, but nobody logs into it, it just runs.

1. Tier: MICRO, not NANO — it's a single feature with no UI, but it needs to run on a schedule and hold a Slack webhook credential, which is more than a disposable one-off script.

2. No mismatch warning needed — the request matches the tier (no mention of user accounts, multiple features, or a database).

3. Minimum stack: a single Node script + a cron trigger (Vercel Cron or a GitHub Action on schedule, no server needed to keep running), env var for the Slack webhook URL. No database — pull the numbers fresh each run, nothing to persist.

4. Upgrade trigger: if this grows to also store historical numbers for trend charts, or gets a dashboard UI beyond the Slack message, that's STANDARD territory — you'd need a real database and probably a small backend at that point.
```

## Why it works
The instinct to reach for a full stack "just in case" wastes setup time on things a MICRO project will never use, and the opposite instinct, building a real product as a throwaway script, creates rework later. Naming the tier out loud makes the scaffolding decision defensible instead of a guess.
