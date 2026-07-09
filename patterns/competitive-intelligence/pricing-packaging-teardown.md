---
title: "Pricing & Packaging Teardown"
slug: "pricing-packaging-teardown"
format: "workflow"
category: "competitive-intelligence"
tools: ["universal"]
difficulty: "intermediate"
est_time: "12 min"
models: ["any"]
summary: "Break down 2-4 competitors' pricing pages into tier structure, packaging levers, and pricing psychology."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're redesigning your own pricing or packaging, or prepping a competitive pricing brief, and need more than a tier price list. You need to see the packaging logic (what actually gates what) and the psychological tricks competitors are using to steer buyers.

## The pattern
```text
Visit the pricing pages of the competitors I list at the bottom of this prompt (2-4 max).

For each one, extract:
1. Tier structure: name, price, billing period (monthly/annual), and the exact metric each tier scales on (seats, usage, API calls, contacts, etc.)
2. Packaging levers: which specific features or limits separate each tier from the one above it. Name the actual gate, not just "more features."
3. Pricing psychology: identify the likely anchor tier (the one designed to look reasonable next to), any decoy tier (overpriced relative to its features, pushing buyers up or down), and any hidden costs (overage fees, setup fees, mandatory add-ons, minimum seats).
4. Self-serve vs. sales-assisted: which tiers can be bought with a credit card and which require "Contact us."

Then compare across all competitors:
- Which packaging lever (seats vs. usage vs. feature-gating) is most common in this market?
- Which competitor gates core functionality most aggressively (e.g. basic security or support behind a high tier)?
- Where is there room for a packaging model none of them are using?

Output as a per-competitor table first, then a 5-bullet synthesis. Only use what's stated on the page; mark anything unclear "Not public."

Competitor pricing page URLs (2-4):
```

## Real example output
| Competitor | Anchor tier | Packaging lever | Notable gate |
|---|---|---|---|
| Competitor A | Growth ($79/mo) | Per-seat + usage cap | SSO gated to Enterprise |
| Competitor B | Team ($99/mo) | Per-seat only | API access gated to Team+ |
| Competitor C | Pro ($49/mo) | Usage-based (API calls) | Support tier gated by plan, not just features |

Synthesis:
- Per-seat pricing is the default lever across all three; none lead with pure usage-based pricing.
- Competitor B's Starter tier looks like a decoy: priced close to Team but excludes API access, which most buyers need on day one.
- Competitor A gates SSO to Enterprise, a common friction point for mid-market buyers who need it earlier than that.
- No competitor offers a usage-based option with no seat minimum — that's an open packaging lane.
- Two of three require a sales call above the entry tier; only Competitor C stays self-serve up to $199/mo.

## Why it works
Naming the actual gate instead of restating marketing copy forces the agent to find the real packaging logic, not just list features. Comparing pricing psychology across competitors surfaces patterns invisible from any single page, and the "open packaging lane" question turns the teardown into a decision input instead of a reference doc.
