---
title: "Turn a Rough Idea into a Structured PRD"
slug: "idea-to-prd"
format: "goal"
category: "product-planning"
tools: ["universal"]
difficulty: "beginner"
est_time: "10 min"
models: ["any"]
summary: "Feed a one-line idea in, get a real PRD out: problem, goals, non-goals, stories, P0/P1/P2."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
---

## When to use this
You have a feature idea in your head or a one-liner from a stakeholder, and you're about to open an AI coding tool to build it. You need a real spec first, not vibes, so the agent builds the right thing on the first pass.

## The pattern
```text
Write a PRD for the idea I paste below the line.

Structure it exactly like this:

1. Problem statement: what's broken today, who feels it, why now.
2. Goals: 3-5 measurable outcomes this feature must achieve.
3. Non-goals: things this explicitly does NOT try to solve. Be specific, this section prevents scope creep later.
4. User stories: as a [user type], I want [action], so that [outcome]. Cover the primary flow and at least one edge case.
5. Requirements, split into:
   - P0 (must ship, feature is broken without it)
   - P1 (should ship, degrades experience if missing)
   - P2 (nice to have, cut first under time pressure)

Keep the whole thing under 500 words. If I don't name a target user below, or the idea is too vague to spec confidently, ask me clarifying questions first, don't guess and fill gaps silently.

My idea:
Target user:
```

## Real example output
**Problem statement**: Users abandon checkout when they can't find how to apply a saved payment method, support tickets about "where do I pay" are up 30% this quarter.

**Goals**: Reduce checkout abandonment by 15%. Cut payment-related support tickets in half. Get saved-method usage to 60% of returning customers.

**Non-goals**: Not building new payment providers. Not redesigning the full checkout flow. Not handling guest checkout changes.

**User stories**: As a returning customer, I want my saved card pre-selected at checkout, so I can pay in one tap. As a user with multiple cards, I want to switch cards without leaving the checkout page...

**Requirements**: P0: pre-select last used card, inline card switcher. P1: card nicknames, expiry warnings. P2: card usage stats in account settings.

## Why it works
Naming the five sections forces the model to separate problem from solution and cut scope explicitly instead of drifting into a wishlist. The non-goals section is what actually saves you from an agent quietly building three extra features you never asked for.
