---
title: "Generate and Rank Headline or Subject Line Variants"
slug: "headline-subject-line-variants"
format: "prompt"
category: "content-writing"
tools: ["universal"]
difficulty: "beginner"
est_time: "10 min"
models: ["any"]
summary: "Get headline options that hook a specific reader trigger, ranked, not ten flavors of the same idea."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["headline-gauntlet-loop", "opening-hook-rewrite", "ab-test-ship-kill-verdict"]
---

## When to use this
You need a headline, subject line, or title. The first thing that comes to mind is flat, but asking an AI for "10 headline ideas" usually produces ten reworded copies of the same angle.

## The pattern
```text
Generate headline options for this piece, each using a DIFFERENT
mechanism to earn a click, not ten rewordings of the same angle.

Use these mechanisms, one headline per mechanism when the supplied facts support it:
1. Specific number or result ("cut X by Y%")
2. Named tension or contradiction ("everyone does X, here's why that's wrong")
3. Direct question the reader is already asking themselves
4. Before/after or transformation framing
5. Blunt, plain statement of the core finding, no hook trick at all

If a mechanism would require an unsupported number, result, or claim, mark it UNAVAILABLE
and replace it with a different evidence-backed angle rather than inventing one.

For each headline, add one line on who it's optimized for (skimmer vs.
committed reader) and what it risks (overpromising, vagueness, etc).

Then rank all 5 for my platform and say which one you'd actually ship
and why; if the platform or audience objective is missing, label the ranking provisional.

First ask me in one message for (1) what the piece actually says, in
1-2 sentences, and (2) the platform (e.g. cold email subject line,
blog title for SEO). Then wait. If I skip the platform, rank for a
blog post title aimed at search traffic.
```

## Real example output
Piece summary: "We switched our onboarding from a 5-step wizard to a single form, and activation went from 34% to 51%."

1. Number: "We Cut Onboarding From 5 Steps to 1 and Activation Jumped 50%" - optimized for skimmers scanning a feed. Risk: reads like every other growth case study headline.
2. Tension: "Your Onboarding Wizard Is Costing You Users, Not Protecting Them" - optimized for readers who already built a multi-step flow and feel defensive about it. Risk: sounds preachy if they didn't build a wizard.
3. Question: "Why Do Users Quit Halfway Through Signup?" - optimized for someone actively searching for this problem right now. Risk: doesn't hint at the answer, lower click-through from readers who think they already know it.
4. Transformation: "We Killed Our 5-Step Wizard. Here's What Happened to Activation." - optimized for narrative case-study readers. Risk: buries the actual number until they click.
5. Plain: "One Form, Not Five Steps: Our Onboarding Rebuild" - optimized for a technical audience that distrusts hook-y titles. Risk: too flat to win a crowded feed.

Ranked for blog SEO traffic: #3 first, it matches actual search queries. #1 second, the concrete number performs well as a secondary meta title. I'd ship #3 as the H1 with #1 as the meta title, they do different jobs.

## Why it works
Assigning a different mechanism per headline forces genuine variation instead of the model's default move of generating five synonyms for the same angle. Naming the risk per option is what turns this from a word game into a real editorial decision.
