---
title: "Landing Page SEO/GEO Audit and Fix List"
slug: "landing-page-seo-geo-audit"
format: "workflow"
category: "seo-geo"
tools: ["claude-code", "codex-cli"]
difficulty: "advanced"
est_time: "20 min"
models: ["any"]
summary: "Audit a landing page's indexability and long-tail targeting, get a prioritized fix list."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
---

## When to use this
A landing page isn't ranking or isn't getting cited by AI answer engines, and you're not sure why. Use this before a rewrite, to find out if the problem is technical (not indexable) or content (wrong keywords) before you touch copy.

## The pattern
```text
Audit a landing page in this repo for SEO and GEO readiness. The page
path and target keyword are at the bottom. If I leave the path empty,
find the site's primary landing page in this repo yourself. Read the
actual file, don't guess.

Check and report on each of these:
1. Title tag: present, under 60 characters, includes the primary keyword
2. Meta description: present, under 160 characters, includes a reason to click
3. Heading structure: exactly one H1, logical H2/H3 nesting, no skipped levels
4. Sitemap: confirm this page's URL appears in the sitemap file if one exists
   in the repo
5. Crawlable links: internal links use real <a href> tags, not JS-only
   onclick handlers
6. Long-tail targeting: does the visible body copy naturally include the
   target keyword and 2-3 related long-tail variants, or only the short
   head term
7. Answer-ability: does the page contain at least one self-contained
   paragraph that directly answers the target keyword as a query in under
   3 sentences, the kind an AI answer engine could quote directly

For each check, mark PASS or FAIL with the specific evidence (actual title
text, actual H1 text, etc). Then output a prioritized fix list, ordered by
impact, with the exact line or file to change for each fix.

Page path in this repo:
Target keyword:
```

## Real example output
```
1. Title tag: FAIL - "Home | Acme" (10 chars, no keyword, generic)
2. Meta description: FAIL - missing entirely
3. Heading structure: PASS - single H1 "Fast Invoicing for Freelancers"
4. Sitemap: PASS - /pricing found in sitemap.xml
5. Crawlable links: FAIL - "See Pricing" button uses onClick router.push,
   no <a href>, at components/Hero.tsx:42
6. Long-tail targeting: FAIL - only "invoicing" appears, no long-tail
   variants like "invoicing for freelance designers"
7. Answer-ability: FAIL - no single paragraph answers the target query
   directly, content is spread across bullet fragments

Prioritized fixes:
1. Add meta description (app/pricing/page.tsx, <head> block) - highest
   impact, currently zero SERP snippet control
2. Replace onClick nav with <Link href> in components/Hero.tsx:42 - blocks
   crawler discovery of the pricing page
3. Rewrite title to include "invoicing for freelancers" (app/pricing/page.tsx:8)
4. Add one direct-answer paragraph near the top of body copy for GEO citability
```

## Why it works
Most ranking failures are one of two root causes: the page is technically uncrawlable, or it targets the wrong keyword granularity. Checking both in one pass, with evidence pulled from the actual file instead of assumptions, tells you which category you're in before you waste time rewriting copy that was never the problem.
