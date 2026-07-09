---
title: "Keyword and Topic Gap Analysis Against Competitors"
slug: "keyword-topic-gap-vs-competitors"
format: "goal"
category: "seo-geo"
tools: ["universal"]
difficulty: "intermediate"
est_time: "20 min"
models: ["any"]
summary: "Find topics competitors rank for that you don't cover, ranked by real opportunity."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You want a content roadmap driven by real gaps, not guesses. Use after pulling your own published topics and a competitor's (from their sitemap, blog index, or a rank-tracking export) — this prompt does the comparison and prioritization, not the data pull.

## The pattern
```text
Here is a list of topics/pages I currently publish on [MY_SITE], and a list
of topics/pages published by competitor(s) [COMPETITOR_NAME(S)]:

My topics:
[MY_TOPIC_LIST]

Competitor topics:
[COMPETITOR_TOPIC_LIST]

Find the gap:
1. List topics the competitor(s) cover that I have no page for at all,
   grouped by theme, not a flat list of 50 items.
2. For each gap, note how many competitors cover it (if more than one was
   given) — topics covered by all competitors and not by me are higher
   priority than one competitor's one-off post.
3. Exclude topics that are off-brand or out of scope for [MY_SITE] given
   what I publish today. Use judgment, don't force every competitor topic
   into the list.
4. For the top 8 gaps, estimate relative opportunity as High/Medium/Low
   based on: how central the topic seems to the competitor's content
   strategy (pillar page vs. one-off post), and how directly it relates to
   my existing topic clusters (easier to rank adjacent to what I already
   cover than to start a brand-new cluster).
5. Output a prioritized table: Topic | Covered by | Opportunity |
   Suggested page type (pillar/comparison/how-to/glossary).
```

## Real example output
```
My topics: invoice automation, payment reminders, AR reporting, API docs
Competitors: InvoiceHQ, PayTrack (both invoicing SaaS)

Gaps found (grouped):
Compliance & tax
- "1099 filing automation" — covered by both competitors, InvoiceHQ has a
  dedicated pillar page with 3 linked sub-guides
- "sales tax nexus for invoicing" — covered by PayTrack only, one blog post

Integrations
- "QuickBooks sync guide" — covered by both, InvoiceHQ ranks #3 for
  "invoicing QuickBooks integration"
- "Stripe vs. manual invoicing" — covered by PayTrack only

Prioritized table:
Topic                          | Covered by        | Opportunity | Page type
1099 filing automation         | InvoiceHQ, PayTrack| High        | Pillar
QuickBooks sync guide          | InvoiceHQ, PayTrack| High        | How-to
Sales tax nexus for invoicing  | PayTrack           | Medium      | Guide
Stripe vs. manual invoicing    | PayTrack           | Medium      | Comparison

Excluded as out of scope: "payroll tax withholding" — InvoiceHQ covers it
but we don't offer payroll features, would be off-brand to rank for.
```

## Why it works
A topic covered by every competitor and missing from your site is a much stronger signal than a single competitor's experimental post — treating them the same wastes writing effort on low-probability bets. Filtering for brand relevance before prioritizing keeps the roadmap from chasing traffic you can't actually convert.
