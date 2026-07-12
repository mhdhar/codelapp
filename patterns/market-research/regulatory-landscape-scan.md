---
title: "Regulatory Landscape Scan for a New Product Launch"
slug: "regulatory-landscape-scan"
format: "workflow"
category: "market-research"
tools: ["universal"]
difficulty: "advanced"
est_time: "20 min"
models: ["any"]
summary: "Map licensing, compliance, and recent policy shifts for a product launch in one structured pass."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
related: ["market-entry-timing-assessment", "macro-trend-relevance-scan", "premortem-until-it-stops-dying"]
---

## When to use this
You're about to launch a product in an industry with regulatory exposure (health, finance, food, transportation, data) and need a first map of what applies before you talk to a lawyer. This is a scoping pass, not legal advice.

## The pattern
```text
Act as a regulatory research analyst.

This is research, not legal advice. Use primary regulator, statute, or agency
sources where available. For every claim, provide the direct source URL,
jurisdiction, and an "as of" date; if those cannot be verified, mark the
claim for counsel rather than treating it as current.

Produce a regulatory landscape scan with these sections:

1. LICENSING & REGISTRATION
   What licenses, permits, or registrations this type of product typically
   requires in this geography. Name the specific regulator or agency for each.

2. CORE COMPLIANCE OBLIGATIONS
   The main rules that would apply once operating (e.g. data handling, consumer
   disclosure, safety standards, advertising restrictions). Group by theme.

3. RECENT POLICY SHIFTS (last 24 months)
   Any new laws, proposed bills, or regulatory guidance changes relevant to
   this product category. Note if something is enacted, pending, or proposed.

4. GRAY AREAS
   Anything where the regulatory treatment is unclear or actively contested
   for products like this. Say plainly "this is unsettled" rather than
   guessing at an answer.

For every claim, cite the specific law, agency, or public source by name and URL.
If you're not certain a regulation applies or is current, flag it as
"verify with counsel" rather than stating it as settled fact. Do not
invent statute numbers or agency names.

End with a prioritized list: which 3 items should be verified with a lawyer
before launch, and why those three first.

Before you do anything, ask me for these in one message and wait: (1) the
product I'm launching, (2) the industry, (3) the geography
(country/state/region).
```

## Real example output
```
Product: Peer-to-peer equipment rental marketplace (power tools, ladders)
Industry: Consumer marketplace / sharing economy
Geography: California, US

LICENSING & REGISTRATION
- Marketplace facilitator registration with California Dept of Tax and Fee
  Administration (CDTFA) for sales/use tax collection on transactions
- No specific "rental marketplace" license identified at state level, but
  local business licenses apply per city of operation (verify per city)

CORE COMPLIANCE OBLIGATIONS
- Consumer protection: California's Unruh Act and general UCL apply to
  marketplace terms and dispute handling
- Insurance/liability: no state mandate for the platform to carry insurance,
  but product liability exposure exists if injury occurs from rented
  equipment (verify with counsel, this is a real exposure area)
- Data privacy: CCPA/CPRA applies given California users, requires privacy
  policy, opt-out mechanisms, and data handling disclosures

RECENT POLICY SHIFTS (last 24 months)
- AB 5 worker classification rules (enacted, ongoing litigation) could
  affect classification if the platform later hires delivery/logistics
  staff, not directly relevant if peer-to-peer only
- Increasing CPRA enforcement activity on marketplace apps re: data sharing
  disclosures (enforcement trend, not new statute)

GRAY AREAS
- Whether the platform bears any liability for defective equipment listed by
  users is unsettled and fact-specific, this is the single biggest legal
  risk area and genuinely contested in similar sharing-economy cases

VERIFY WITH COUNSEL, IN ORDER
1. Liability exposure for injury from rented equipment (biggest financial
   risk, currently a gray area)
2. CCPA/CPRA compliance obligations before collecting any user data
3. Local business licensing requirements per city of first launch
```

## Why it works
Splitting settled law from recent shifts from genuine gray areas stops the model from flattening everything into false certainty. The "verify with counsel" flag on uncertain claims keeps this a research starting point, not a substitute for legal review.
