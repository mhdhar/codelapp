---
title: "Positioning Gap Analysis"
slug: "positioning-gap-analysis"
format: "workflow"
category: "competitive-intelligence"
tools: ["universal"]
difficulty: "advanced"
est_time: "15 min"
models: ["any"]
summary: "Compare your product against competitor URLs to find the gaps worth leading with in messaging."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
related: ["competitor-positioning-matrix-loop", "competitor-messaging-audit", "category-creation-vs-incumbent-framing"]
---

## When to use this
You're writing a landing page, pitch deck, or sales battlecard and need to know what to say that's true, differentiated, and not just "we're better." Use this after you already have a rough list of competitors, before you write a word of copy.

## The pattern
```text
Visit each competitor URL I give you and read their homepage, pricing, and features pages. Then:

1. List what every competitor in this set publicly states in common (the table stakes, don't lead with these), with the source URL for each claim.
2. List what each competitor does not mention or underemphasizes on the reviewed pages. Do not treat page silence as a product capability gap.
3. Cross-reference those documented messaging gaps against my product description to find where I may have a real, verifiable advantage.
4. Rank the top 3 as buyer-relevance hypotheses, based on buyer evidence I provide; if that evidence is absent, say the ranking is unvalidated rather than treating novelty as importance.

For each of the top 3, write one sentence of messaging that states the gap as a customer benefit, not a feature brag. Flag anything where my product description or the reviewed competitor pages do not give you enough to confirm the claim. State the pages and date reviewed.

Before you visit anything, ask me in one message and wait: (1) my product, two or three lines on what it does and for whom, (2) the competitor URLs.
```

## Real example output
Table stakes across all 3 competitors: SSO, REST API, Slack integration, 99.9% uptime SLA. Don't lead with these.

Gaps found:
1. None of the three publish a self-serve migration tool. Two require a sales call to switch off a competitor.
2. Competitor A and C have no visible audit log feature. Competitor B has it but gated to Enterprise only.
3. All three price per-seat with no usage-based option, which penalizes teams with spiky usage.

Top 3 ranked messaging:
1. "Migrate in an afternoon, not a sales cycle." (self-serve migration, high buyer relevance, low competitor coverage)
2. "Audit logs on every plan, not just Enterprise." (needs confirmation: does the product description actually include this on lower tiers?)
3. "Pay for what you use, not how many people are logged in." (usage-based pricing, medium relevance, depends on buyer segment)

## Why it works
Table stakes get filtered out first so the agent doesn't waste a differentiator slot on something every competitor already has. Ranking by buyer relevance, not just novelty, stops you from leading with a gap nobody cares about. The confirmation flag catches claims your own product description doesn't actually support.
