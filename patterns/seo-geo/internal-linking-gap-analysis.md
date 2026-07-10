---
title: "Internal Linking Gap Analysis"
slug: "internal-linking-gap-analysis"
format: "workflow"
category: "seo-geo"
tools: ["universal"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Find orphaned pages and thin topic clusters your internal links never reach."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["broken-internal-link-fix-loop", "sitemap-priority-for-large-sites", "content-decay-audit"]
---

## When to use this
After publishing a batch of new content, or when a page with genuinely good content isn't ranking and you suspect it has no internal link equity flowing to it. Use with an export of your site's pages and their current internal outlinks.

## The pattern
```text
I need an internal linking analysis of my site. The input is a page
inventory: each page with its topic and current outbound internal links
(which other pages on the site it links to), plus organic traffic per
page where I have it.

Analyze internal linking:
1. Identify orphan pages: pages that no other page links to. List them.
2. Group pages into topic clusters by subject matter. For each cluster,
   identify a natural "hub" page (the broadest or highest-traffic page on
   that topic) and list which cluster members it currently does NOT link
   to but should.
3. Flag pages with fewer than 2 incoming internal links; they're at risk
   even if not fully orphaned.
4. For each gap found in steps 1-3, suggest the specific source page,
   target page, and anchor text to use. Anchor text should describe the
   target page's content, not say "click here" or "read more."
5. Do not suggest a link between two pages with no topical relationship
   just to fix an orphan; that's a spam pattern. If an orphaned page has
   no natural topical home, flag it separately as "needs a new hub"
   instead of forcing a link.

Output the gap list sorted by page importance (use organic traffic if
given in the data, otherwise use position in the site's primary nav).

If you're running inside the site's codebase, build the inventory yourself
from the routes, content files, and the links in them. Otherwise, first
ask me to paste the page and link inventory, then wait for my reply.
```

## Real example output
```
Orphans (0 incoming internal links):
- /docs/api/rate-limits - no page links here, only reachable via direct
  URL or the sitemap
- /blog/case-study-northwind - published 3 months ago, never linked from
  /blog or /customers

Cluster: "API integration" - hub = /docs/api/overview (highest traffic
in cluster: 2,100 sessions/mo)
Hub is missing links to: /docs/api/rate-limits, /docs/api/webhooks

Cluster: "Customer stories" - hub = /customers
Hub is missing links to: /blog/case-study-northwind

Under-linked (1 incoming link):
- /pricing/enterprise - only linked from /pricing, not from /features or
  the main nav

Gap fixes:
1. Add link from /docs/api/overview to /docs/api/rate-limits, anchor text
   "API rate limits and quotas"
2. Add link from /customers to /blog/case-study-northwind, anchor text
   "how Northwind cut invoice processing time 40%"
3. Add link from /features/enterprise-security to /pricing/enterprise,
   anchor text "enterprise pricing"

No natural hub found for: /blog/2024-year-in-review - flagged as "needs
a new hub," doesn't fit an existing cluster, consider linking from a
future /blog/archive index instead of forcing it into an unrelated page.
```

## Why it works
Search engines and AI crawlers weight pages partly by how much internal link equity points at them; a page nobody links to reads as unimportant even if the content is strong. Grouping by topic cluster before suggesting links keeps the fixes relevant instead of manufacturing links that look like spam.
