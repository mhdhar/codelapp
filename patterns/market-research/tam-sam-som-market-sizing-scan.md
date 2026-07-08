---
title: "TAM/SAM/SOM Market Sizing Scan"
slug: "tam-sam-som-market-sizing-scan"
format: "workflow"
category: "market-research"
tools: ["universal"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Scope TAM/SAM/SOM for a product idea in one pass, with every number labeled estimate or sourced."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
---

## When to use this
You have a product idea and need a first-pass market size before pitching, prioritizing, or writing a business case. You don't need a consultant deck, you need defensible numbers fast.

## The pattern
```text
Act as a market sizing analyst. I'm evaluating this product idea:

[PRODUCT DESCRIPTION]

Target customer: [WHO BUYS THIS]

Do the following:

1. Define TAM, SAM, and SOM for this product in plain terms specific to this
   market. Don't use generic definitions, tie each one to this product.

2. For each of TAM, SAM, SOM, give a number (or range) and show your math:
   what inputs you multiplied or divided, and where each input came from.

3. Tag every input number as one of:
   - SOURCED: cites a specific public report, government data set, or
     company disclosure (name it, even if you can't give a live link)
   - ESTIMATE: your own reasoning or extrapolation, state the assumption
     plainly (e.g. "assuming 5% of SMBs in this category would adopt")

4. List the 3 assumptions that most change the outcome if wrong, and show
   what SOM looks like at a pessimistic and optimistic version of each.

5. End with a "confidence" line: how much you'd trust this number for a
   funding pitch vs. a rough internal sanity check, and what research would
   tighten it most.

Do not present estimates as sourced facts. If you don't know a number,
say so and estimate transparently rather than inventing precision.
```

## Real example output
```
Product: AI-powered scheduling assistant for independent dental practices (US)

TAM: All US dental practices that could theoretically use scheduling software
  ~198,000 practices (SOURCED: ADA Health Policy Institute practice counts)
  x avg $3,000/yr willingness to pay (ESTIMATE, based on comparable practice
  management software pricing tiers)
  = ~$594M TAM

SAM: Independent/small group practices (1-3 locations) that are not already
  locked into a bundled practice management suite
  ~70% of practices are independent (SOURCED: ADA practice ownership survey)
  x 40% not on a bundled all-in-one suite (ESTIMATE, based on market share of
  top 3 incumbent suites covering ~60% of practices)
  = ~55,000 practices x $3,000 = ~$166M SAM

SOM (3-year realistic): 
  0.5-1.5% of SAM captured = 275-825 practices = $825K-$2.5M ARR

Assumptions that move the number most:
1. Willingness to pay ($3,000 vs $1,500/yr halves everything downstream)
2. Bundled suite penetration (if it's 75% not 60%, SAM roughly doubles)
3. Sales motion (self-serve vs. outbound changes realistic SOM capture rate
   by 3-5x)

Confidence: Good enough for internal prioritization, not pitch-ready. The ADA
counts are solid, the willingness-to-pay and bundling numbers are estimates
that need 5-10 real customer conversations to tighten before using this in
a fundraising deck.
```

## Why it works
Forcing a SOURCED/ESTIMATE tag on every number stops the model from blending real data with confident-sounding guesses. Asking for the assumptions that move the outcome most turns a static number into something you can actually stress-test before you rely on it.
