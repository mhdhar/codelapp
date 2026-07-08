---
title: "Turn a Rough Campaign Idea Into a Creative Brief"
slug: "campaign-idea-to-creative-brief"
format: "workflow"
category: "marketing-growth"
tools: ["claude-code", "universal"]
difficulty: "intermediate"
est_time: "20 min"
models: ["any"]
summary: "Expand a one-line campaign idea into a full brief an agent or agency can execute against."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
---

## When to use this
You have a campaign idea in your head ("do something around back-to-school" or "push our new pricing tier") but nothing written down, and a creative team or AI agent needs an actual brief before they can start producing assets.

## The pattern
```text
Step 1: Dump the raw idea.
Paste everything you know, unfiltered, even if it's messy:
Idea: [ROUGH CAMPAIGN IDEA]
Context: [WHY NOW, ANY CONSTRAINTS, BUDGET/TIMELINE IF KNOWN]

Step 2: Ask the agent to draft the brief.
Turn the above into a creative brief with these exact sections:
- Objective: one sentence, one measurable outcome, not a vibe.
- Audience: who specifically this is for. If the idea doesn't specify, ask me before guessing.
- Key message: the single sentence a viewer should remember. Not a list of features.
- Channels: where this runs, ranked by priority, with a one-line reason for each.
- Success metric: the one number that tells us if this worked, plus the target value.
- Out of scope: what this campaign explicitly does not try to do.

Keep every section under 4 sentences. Flag any section where you had to guess instead of using something I told you, using [ASSUMPTION: ...].

Step 3: Review the [ASSUMPTION] flags.
Read the brief back. For every [ASSUMPTION] tag, either confirm it or correct it, then ask the agent to regenerate just that section.

Step 4: Hand off.
Once no [ASSUMPTION] tags remain, the brief is ready to hand to a creative team or a content-generation agent as-is.
```

## Real example output
- Objective: Drive 500 trial signups from the new self-serve pricing tier within 30 days of launch.
- Audience: Existing free-tier users who've hit the usage cap at least twice in the last 30 days. [ASSUMPTION: inferred from "push our new pricing tier," confirm this is the target segment.]
- Key message: You've already outgrown free. Upgrading takes two minutes.
- Channels: 1) In-app upgrade prompt (highest intent, already-engaged users), 2) Email to capped users (direct, low cost), 3) X thread (awareness, lower priority for this objective).
- Success metric: 500 trial signups attributed to this campaign by day 30.
- Out of scope: New-user acquisition, brand awareness, paid ads.

## Why it works
Splitting the workflow into draft-then-review with explicit [ASSUMPTION] tags stops the agent from silently inventing an audience or metric. You only spend review time on the parts it actually guessed at, not the whole document.
