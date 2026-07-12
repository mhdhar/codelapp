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
related: ["keyword-topic-gap-vs-competitors", "internal-linking-gap-analysis", "canonical-duplicate-content-resolver"]
---

## When to use this
Monthly or quarterly, when Search Console shows a set of pages trending down in clicks or average position. Use this to sort "needs a refresh" from "lost to a real competitor" from "cannibalized by our own page" before spending writing time on the wrong fix.

## The pattern
```text
I have Search Console / analytics data for pages on my site, showing URL,
clicks, impressions, and average position for the current period vs. the
prior period of the same length. Treat the window as the last 90 days vs.
the prior 90 unless the data says otherwise.

For each page showing a decline (clicks or position down at least 15%):
1. Classify the observed pattern using only the data given, and label every
   cause below as a hypothesis until confirmed with query-level, page, or SERP
   evidence:
   - STALE HYPOTHESIS: impressions flat or down, position down; confirm the
     page has dated or outdated content and compare current competing results
   - CANNIBALIZATION HYPOTHESIS: impressions flat, but clicks/position down
     while another of my own pages on a similar topic gained; identify the
     competing URL and confirm query overlap
   - SERP/DEMAND CHANGE HYPOTHESIS: impressions themselves dropped sharply;
     inspect the affected queries and current SERP before attributing this to
     an AI Overview, featured snippet, or any other feature
   - SEASONAL: this pattern matches a prior-year dip for the same period,
     if that's inferable from the data
   - UNKNOWN: doesn't clearly match the above; say so rather than forcing
     a category
2. For STALE HYPOTHESES, name the specific signal only when the page metadata
   or content was supplied; otherwise say that a page review is required.
3. For CANNIBALIZATION HYPOTHESES, name the specific other URL and the query
   overlap evidence, or say it remains unconfirmed.
4. Output a prioritized refresh list: URL | Decay type | Evidence | Clicks
   lost (absolute number) | Recommended action, sorted by clicks lost.

First ask me to paste the traffic data, then wait. Don't start until you
have it.
```

## Real example output
```
Page: /blog/best-invoicing-software-2024
Current: 340 clicks, 12,000 impressions, avg position 8.2
Prior: 890 clicks, 11,800 impressions, avg position 4.1
-> STALE HYPOTHESIS. Impressions held steady but position dropped 4 spots.
Title and H1 both say "2024" and it's now mid-2026, a clear page-level
staleness signal. Confirm current competing results before attributing loss.
Clicks lost: 550.
Action: Update title/H1 to current year, refresh the comparison table with
current pricing, republish with a real updated date.

Page: /features/invoice-reminders
Current: 210 clicks, 6,400 impressions, avg position 6.5
Prior: 480 clicks, 6,300 impressions, avg position 3.9
Also in dataset: /features/automated-payment-reminders launched 4 months
ago, current 410 clicks, avg position 2.1, same core topic.
-> CANNIBALIZATION HYPOTHESIS. The newer page covers the same topic and is
performing better, but query-level overlap still needs confirmation.
Clicks lost: 270.
Action: Compare Search Console query data for both URLs. Only then decide
whether to consolidate or differentiate them around distinct intent.

Page: /docs/getting-started
Current: 1,100 clicks, 41,000 impressions, avg position 2.8
Prior: 1,300 clicks, 61,000 impressions, avg position 2.6
-> SERP/DEMAND CHANGE HYPOTHESIS. Position barely moved but impressions
dropped 33%. The data does not identify whether demand changed or which SERP
feature, if any, affected clicks.
Clicks lost: 200.
Action: Inspect the affected queries and current SERP before changing the
page or choosing a content response.

Prioritized refresh list:
URL                                    | Type         | Clicks lost
/blog/best-invoicing-software-2024     | STALE HYPOTHESIS | 550
/features/invoice-reminders            | CANNIBALIZATION HYPOTHESIS | 270
/docs/getting-started                  | SERP/DEMAND CHANGE HYPOTHESIS | 200
```

## Why it works
"Traffic is down" has several possible causes, and each needs different evidence before a fix. Classifying the observed pattern first stops you from refreshing, consolidating, or blaming a SERP feature before the data supports it.
