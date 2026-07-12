---
title: "Zero-Loss Redirect Map for a Site Migration"
slug: "redirect-map-site-migration"
format: "loop"
category: "seo-geo"
tools: ["universal"]
difficulty: "advanced"
est_time: "30 min"
models: ["any"]
summary: "Map every old URL to its new home, then loop until there are no chains, loops, or lazy homepage dumps."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["broken-internal-link-fix-loop", "canonical-duplicate-content-resolver", "rollback-readiness-check"]
---

## When to use this
You're changing URL structure, merging sites, or replatforming, and every old URL needs a 301 target before launch. Use this before the migration ships. A redirect map written by hand the night before is where rankings go to die.

## The pattern
```text
I'm migrating my site and need a 301 redirect map. The input is two
lists: every old URL on my site, then a line reading "NEW URLS:", then
every URL that will exist after the migration.

Build the map in this loop:
1. Match each old URL to the single best new URL by slug, path segments,
   and apparent topic. Tag each match HIGH, MEDIUM, or LOW confidence.
2. Move every LOW match to a "needs human decision" list with your top 2
   candidate targets and one line on why you couldn't decide. Never guess
   on a LOW or include it in the redirect map until it is decided.
3. Run these checks on the map:
   - Every old URL appears exactly once across the redirect map and the
     needs-human list.
   - No target is itself an old URL that redirects (that's a chain).
   - No pair of rules sends A to B and B back to A (that's a loop).
   - No target is the homepage unless the old page truly has no
     equivalent. Flag every homepage target as suspect.
   - No old URL appears in both the map and the needs-human list.
4. If any check fails, fix the map and run all of step 3 again. Repeat
   until every check passes in a single pass.
5. Output: (a) the passing map as CSV with columns old_url,new_url,status
   unless I name a server format like nginx or a Next.js redirects block,
   (b) the needs-human list, (c) counts: total old URLs, HIGH, MEDIUM,
   needs-human, homepage-flagged.

First ask me to paste the old URLs, then a line reading "NEW URLS:", then
the new URLs, all in one message. Wait until you have both lists.
```

## Real example output
```
Checks: pass on iteration 2 (iteration 1 found 3 chains where a target
was itself in the old list, and 1 A->B->A loop between /pricing and
/plans; all re-pointed to final destinations).

old_url,new_url,status
/blog/2023/invoice-tips,/resources/invoice-tips,301
/features.html,/features,301
/docs/api/v1/auth,/docs/api/authentication,301
/pricing-2024,/pricing,301

Needs human decision (4):
- /blog/founder-interview-2022 -> /about/team OR /resources/archive
  (topic exists in neither; page reads as PR, not evergreen content)
- /labs/beta-signup -> /product/early-access OR drop with 410
  (beta program appears discontinued in the new URL list)

Counts: 312 old URLs. 268 HIGH, 40 MEDIUM, 4 needs-human,
2 homepage-flagged (both legacy campaign pages with no new equivalent).
```

## Why it works
Redirect maps fail on structure, not on individual picks: chains leak equity hop by hop, loops kill pages outright, and "just point it at the homepage" is a soft 404 in Google's eyes. Making the model re-run the full check suite after every fix, instead of patching one error and declaring victory, is what catches the second-order breakage a single pass always misses.
