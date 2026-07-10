---
title: "Competitor Feature Scan"
slug: "competitor-feature-scan"
format: "workflow"
category: "competitive-intelligence"
tools: ["universal"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "Turn a competitor's public pages into a structured pricing and feature comparison table."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
related: ["pricing-packaging-teardown", "keyword-topic-gap-vs-competitors", "competitor-watch-loop"]
---

## When to use this
You're prepping for a sales call, a board update, or a roadmap decision and need a current, accurate read on what a competitor offers and charges. Skip the manual page-by-page note-taking.

## The pattern
```text
Visit the competitor site I give you, plus its pricing, features, and product pages. Build a structured comparison from what you find.

Extract:
1. Pricing tiers: name, price, billing period, what's included at each tier
2. Core features: list every distinct feature mentioned, grouped by category (e.g. core, integrations, security, support)
3. Positioning: the exact language they use to describe who the product is for and what problem it solves
4. Notable gaps: anything a competitor in this space would typically have that's missing or not mentioned

Output as a markdown table with columns: Feature/Tier | Details | Source page (URL).

Only include what's stated on the pages. Do not infer or guess capabilities. If a price or detail isn't public, mark it "Not public."

Start by asking me for the competitor's URL, then wait for my reply before visiting anything.
```

## Real example output
| Feature/Tier | Details | Source page |
|---|---|---|
| Starter | $29/mo, billed monthly. 3 seats, 10k API calls/mo, email support | /pricing |
| Growth | $99/mo, billed monthly or $79/mo annual. 15 seats, 100k API calls/mo, Slack support | /pricing |
| Enterprise | Custom pricing. Unlimited seats, SSO, dedicated CSM | /pricing |
| SSO/SAML | Enterprise tier only | /pricing, /security |
| Webhooks | Growth tier and above | /features |
| Audit logs | Not public | n/a |

Positioning: "Built for engineering teams who ship fast and can't afford downtime." Targets mid-market eng orgs, not solo devs.

Notable gaps: no mention of a public API rate-limit page, no SOC 2 badge visible, no mobile app.

## Why it works
Constraining the agent to "only what's on the page" prevents hallucinated feature claims. The source-URL column makes every line auditable, so you can hand this straight to sales or leadership without re-verifying it yourself.
