---
title: "Canonical Tag and Duplicate Content Resolver Loop"
slug: "canonical-duplicate-content-resolver"
format: "loop"
category: "seo-geo"
tools: ["universal"]
difficulty: "advanced"
est_time: "25 min"
models: ["any"]
summary: "Cluster near-duplicate pages, pick one canonical each, and stop them competing."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["redirect-map-site-migration", "content-decay-audit", "sitemap-priority-for-large-sites"]
---

## When to use this
Search Console shows multiple URLs on your site weakly ranking for the same query, or you suspect parameter, filter, or print URLs are splitting authority. Use this loop before merging or redirecting anything, so you don't collapse a page that actually has unique value into one that doesn't.

## The pattern
```text
I have a list of URLs on my site that may be duplicate or near-duplicate
content. Each row has the URL, title, word count, a short content summary,
and organic traffic where I have it.

Run this loop on that list:
1. Cluster the URLs into groups that target the same search intent. State
   the shared intent for each cluster in one line.
2. For each cluster with more than one URL, check for genuinely unique
   content in every URL before deciding it's a true duplicate: different
   data, different examples, different sub-topic. List what's unique, if
   anything, per URL.
3. If a cluster is a true duplicate (no meaningfully unique content across
   URLs), pick ONE canonical URL based on, in order: highest current
   organic traffic, then shortest/cleanest URL path, then most complete
   content. State which rule decided it.
4. If a cluster has real differences, do NOT canonicalize. Flag it as
   "needs differentiation" and say what would need to change in the
   non-canonical URLs to justify them existing separately.
5. For every true-duplicate cluster, output the exact <link rel="canonical">
   tag to add to each non-canonical URL, pointing at the chosen canonical.
6. Flag any canonical decision where a non-canonical URL has MORE organic
   traffic than the chosen canonical; that's a signal to re-check the pick
   before applying it.

Do not canonicalize a cluster until step 2 is complete for every URL in it.

First ask me to paste the URL list, then wait. If my rows are missing
traffic numbers, skip the traffic rule in step 3 and decide on URL path
and completeness instead.
```

## Real example output
```
Cluster 1 - intent: "compare pricing plans"
URLs: /pricing, /plans, /pricing-2024
Step 2: /pricing (890 words, current tiers, live); /plans (901 words,
identical tier copy, same table, no unique content); /pricing-2024
(612 words, stale Q1 2024 prices, no unique content)
Step 3: True duplicate. Canonical = /pricing (highest traffic: 4,100
sessions/mo vs. 310 for /plans and 40 for /pricing-2024).
Step 5: Add <link rel="canonical" href="https://acme.com/pricing"> to
/plans and /pricing-2024.
Step 6: No flag - /pricing already has the most traffic.

Cluster 2 - intent: "invoice automation overview"
URLs: /features/invoice-automation, /solutions/invoicing
Step 2: /features/invoice-automation (1,200 words, product screenshots,
setup steps); /solutions/invoicing (980 words, industry use-case framing
for accounting firms, different examples, distinct CTA)
Step 4: NEEDS DIFFERENTIATION, not a true duplicate. /solutions/invoicing
should be rewritten to lead with the accounting-firm use case explicitly
in its title and H1 instead of restating the general feature description.
```

## Why it works
Canonicalizing on instinct kills pages that were actually serving a different intent, which loses real traffic instead of consolidating it. Requiring evidence of uniqueness before merging, and flagging traffic mismatches before they get applied, catches both failure modes: false duplicates and a wrong canonical pick.
