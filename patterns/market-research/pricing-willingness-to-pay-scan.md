---
title: "Pricing & Willingness-to-Pay Scan for a New Category"
slug: "pricing-willingness-to-pay-scan"
format: "workflow"
category: "market-research"
tools: ["universal"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Estimate a defensible price range for a new product category using proxy signals, not guesses."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're setting a first price for a product in a category with no direct comps (new category, first mover, or repositioned offer) and need a defensible starting band before you build a pricing page or run a survey.

## The pattern
```text
Act as a pricing research analyst.

Do the following:

1. ANCHOR COMPS: list 3-5 adjacent products (not direct competitors, since none
   exist) that this buyer already pays for and that solve a similar job. For
   each, give the price point and tag it:
   - SOURCED: cites a specific public pricing page or disclosure (name it)
   - ESTIMATE: inferred from category norms, state the assumption plainly

2. VALUE METRIC: propose 2-3 candidate metrics this product could price on
   (per seat, per usage, flat fee, per outcome). Say which one matches how
   this buyer already thinks about value in this category, and why.

3. PRICE BAND: using the anchors and value metric, propose a low/mid/high
   price band. Show the reasoning, not just the number.

4. WTP RISK FACTORS: name 2-3 things specific to this buyer or category that
   could make real willingness-to-pay lower than the anchor comps suggest
   (e.g. no existing budget line for this, feature reads as "nice to have").

5. VALIDATION METHOD: name the cheapest fast way to test this band before
   committing (e.g. stating the anchor price in 5-10 real sales conversations,
   a pricing-page A/B, a Van Westendorp survey).

Tag every price number as SOURCED or ESTIMATE. Do not state a competitor's
price unless you can say where it came from. If you don't have a real comp,
say so and estimate transparently instead of inventing a number.

End with the single price you'd start with for a v1 launch, and the specific
signal that would make you revise it within 30 days.

Start by asking me in one message for: (1) the product, (2) the target
customer, (3) the closest comparable products, imperfect comps are fine.
Wait for my reply. If I skip the comps, propose the adjacent categories
yourself and say you did.
```

## Real example output
```
Product: AI-generated deposition summary tool for solo/small-firm litigators
Target: attorneys who currently pay a court reporter for a transcript and
  manually summarize it themselves

ANCHOR COMPS
- Court reporter transcript fee: ~$3.50-$5.00/page, avg deposition transcript
  runs 150-250 pages = $500-$1,200 per deposition (SOURCED: range cited by
  state court reporter associations and law firm billing guides)
- Otter.ai Business: $20/user/month (SOURCED: Otter.ai public pricing page)
- CaseFleet-style case organization software: $49-$99/user/month (SOURCED:
  public pricing pages)
- Westlaw/Lexis transcript-analytics add-on tier: ~$100-250/month per user
  (ESTIMATE, inferred from published bundle tiers, exact add-on price is not
  public)

VALUE METRIC
Per-deposition fee matches how this buyer already pays (transcript cost is
billed per case event, not monthly). Per-seat monthly was considered but
rejected: usage is lumpy, some months zero depositions, some months five.

PRICE BAND
Low: $25/deposition (reads as a cheap add-on to an existing case cost)
Mid: $75/deposition (roughly 10-15% of the transcript fee, positioned as
  cheap insurance against manual summary time)
High: $150/deposition (still under 30% of transcript cost, the ceiling
  before it starts competing with the transcript line item itself)

WTP RISK FACTORS
- Attorneys bill transcript review time to the client, so this tool competes
  with billable hours, not just out-of-pocket cost. Some may resist paying to
  reduce their own billable time. Real adoption risk, not just a pricing one.
- No existing budget line item for "AI summary tool," it would need to be
  absorbed into case costs or the firm's overhead, likely slows adoption at
  solo-practitioner scale specifically.

VALIDATION METHOD
Call or email 8-10 solo/small-firm litigators, state the $75/deposition
anchor directly, ask if it reads as obviously cheap, fair, or too expensive.
Five "obviously cheap" answers before any "too expensive" is a signal to
test $100-125 instead.

START AT: $75/deposition for v1. Revise within 30 days if the first 10 real
conversations skew toward "obviously cheap" (test $100+), or if adoption
stalls specifically on "no budget for this" (test a flat monthly retainer
instead of per-deposition pricing).
```

## Why it works
Anchoring to what the buyer already pays for adjacent jobs, not to a feature list, grounds the price in real budget behavior instead of internal cost-plus math. The SOURCED/ESTIMATE tag on every comp keeps you from treating a guess about a competitor's price as fact.
