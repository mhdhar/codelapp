---
title: "Content Decay Audit: Which Pages Are Losing Rank and Why"
slug: "content-decay-audit"
format: "workflow"
category: "seo-geo"
tools: ["universal"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Diagnose why specific pages are sliding in traffic before you spend time rewriting them."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
Monthly or quarterly, when Search Console shows a set of pages trending down in clicks or average position. Use this to sort "needs a refresh" from "lost to a real competitor" from "cannibalized by our own page" before spending writing time on the wrong fix.

## The pattern
```text
Here is Search Console / analytics data for pages on [SITE_URL] over the
last [TIME_PERIOD], showing URL, clicks, impressions, and average position
for the current period vs. the prior period of the same length:

[TRAFFIC_DATA]

For each page showing a decline (clicks or position down at least 15%):
1. Classify the likely decay cause using only the data given:
   - STALE: impressions flat or down, position down — content likely
     outdated versus fresher competing results
   - CANNIBALIZED: impressions flat, but clicks/position down while
     another of my own pages on a similar topic gained — check the list
     for a page covering similar ground
   - SERP CHANGE: impressions themselves dropped sharply — likely a new
     SERP feature (AI Overview, featured snippet) taking the click, not
     a ranking loss
   - SEASONAL: this pattern matches a prior-year dip for the same period,
     if that's inferable from the data
   - UNKNOWN: doesn't clearly match the above — say so rather than forcing
     a category
2. For STALE pages, name the specific signal of staleness if inferable (a
   date in the title/content, a "last updated" mismatch, outdated stats).
3. For CANNIBALIZED pages, name the specific other URL competing for it.
4. Output a prioritized refresh list: URL | Decay type | Evidence | Clicks
   lost (absolute number) | Recommended action, sorted by clicks lost.
```

## Real example output
```
Page: /blog/best-invoicing-software-2024
Current: 340 clicks, 12,000 impressions, avg position 8.2
Prior: 890 clicks, 11,800 impressions, avg position 4.1
-> STALE. Impressions held steady but position dropped 4 spots. Title and
H1 both say "2024" and it's now mid-2026 — clear staleness signal, likely
losing to freshly-dated competing roundups.
Clicks lost: 550.
Action: Update title/H1 to current year, refresh the comparison table with
current pricing, republish with a real updated date.

Page: /features/invoice-reminders
Current: 210 clicks, 6,400 impressions, avg position 6.5
Prior: 480 clicks, 6,300 impressions, avg position 3.9
Also in dataset: /features/automated-payment-reminders launched 4 months
ago, current 410 clicks, avg position 2.1, same core topic.
-> CANNIBALIZED by /features/automated-payment-reminders. The newer page
is winning the same query.
Clicks lost: 270.
Action: Consolidate — canonicalize /features/invoice-reminders to the
newer page, or differentiate it to target a distinct sub-intent instead
of competing head-on.

Page: /docs/getting-started
Current: 1,100 clicks, 41,000 impressions, avg position 2.8
Prior: 1,300 clicks, 61,000 impressions, avg position 2.6
-> SERP CHANGE. Position barely moved but impressions dropped 33% —
consistent with an AI Overview now answering "how do I get started with
[product]" directly in the SERP without a click-through.
Clicks lost: 200.
Action: Not a content problem. Strengthen the page's citability so it's
the source the AI Overview pulls from (see citation-worthiness audit).

Prioritized refresh list:
URL                                    | Type         | Clicks lost
/blog/best-invoicing-software-2024     | STALE        | 550
/features/invoice-reminders            | CANNIBALIZED | 270
/docs/getting-started                  | SERP CHANGE  | 200
```

## Why it works
"Traffic is down" has at least four different root causes and each one needs a different fix — rewriting a page that's actually being cannibalized by your own newer page wastes the effort twice. Classifying decay type from the data before touching copy stops you from refreshing a page that was never stale.
