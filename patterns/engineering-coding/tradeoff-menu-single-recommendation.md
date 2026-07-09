---
title: "Tradeoff Menu, Single Recommendation"
slug: "tradeoff-menu-single-recommendation"
format: "prompt"
category: "engineering-coding"
tools: ["universal"]
difficulty: "beginner"
est_time: "5 min"
models: ["any"]
summary: "Force one clear recommendation with a one-line tradeoff instead of a noncommittal list of options."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
An agent keeps handing you 3-4 options with pros and cons and no actual opinion, leaving the decision back on your plate. Use this whenever you need a real recommendation, not a menu.

## The pattern
```text
I need a decision, not a menu. If I list options with my question, treat those as the options on the table; if I don't, propose them yourself.

1. If there are more than 2 genuinely reasonable options, narrow it to the top 2 — discard options that are clearly worse on every axis.
2. For each of the (at most 2) remaining options, state the single biggest tradeoff in one sentence, not a bullet list of pros and cons.
3. Recommend exactly one. State the recommendation before the reasoning, not after.
4. Give the one-sentence reason you'd pick it over the other, specific to my actual situation, not a generic "it depends."
5. Do not say "both are valid" as a final answer. If it truly is a coin flip, say that explicitly and tell me what additional information would break the tie.

First ask me for the question, then wait. Don't answer until you have it.
```

## Real example output
```
> The question: Should the new notifications feature use WebSockets or polling?

Recommendation: polling, every 15 seconds.

Reasoning: your notification volume is low (avg 2-3 per user per day per the current email-notification data) and you don't have a WebSocket server running anywhere else in the stack — polling gets you live-enough updates with zero new infrastructure, while WebSockets would mean standing up and operating a stateful connection layer for a feature that doesn't need sub-second latency.

Tradeoff if you went WebSockets instead: near-instant delivery, at the cost of a new always-on service to deploy, monitor, and scale separately from your otherwise-stateless API.

Not a coin flip here — this only flips if notification volume or latency requirements change materially (e.g. if this becomes a chat feature, not a notification feature).
```

## Why it works
A list of options with pros and cons quietly transfers the decision back to you while looking like it did the analysis. Requiring the recommendation first, one sentence of reasoning tied to your actual numbers, and a ban on "it depends" turns analysis paralysis into a usable answer.
