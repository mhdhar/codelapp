---
title: "Turn Your Product's Real Activation Steps Into an Onboarding Email Sequence"
slug: "onboarding-sequence-from-activation-steps"
format: "prompt"
category: "marketing-growth"
tools: ["universal"]
difficulty: "beginner"
est_time: "10 min"
models: ["any"]
summary: "Give it your actual activation funnel steps, get one nudge email per step, not a generic welcome series."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["retention-cohort-lever-review", "empty-state-design-pass", "cold-outreach-sequence-from-icp"]
---

## When to use this
You know the exact steps a user takes to reach "activated" in your product (your real funnel, not a guess) and need an onboarding email sequence that nudges each specific step, instead of a generic "welcome, here's a video tour" series nobody opens past email one.

## The pattern
```text
Write an onboarding email sequence, one email per activation step, from the product and funnel I give you.

For each step except the final activation event, write one email triggered when the user has not completed that step within a window supported by the funnel/event data I give you; if no timing data exists, label the proposed cadence `TEST WINDOW` in the trigger line rather than stating it as established:
- Subject line under 50 characters, naming the specific step, not "getting started" or "don't forget."
- Body under 150 words.
- Exactly one CTA, linking to the precise in-product action for that step, not the homepage.
- If this step is the known highest drop-off point and I give you evidence of the blocker, add a P.S. addressing that blocker in plain language; otherwise omit the P.S.

Rules: no countdown timers, no "last chance," no "just checking in." Write like someone who's used this product, not a drip-campaign template. Do not invent setup times, retention effects, or other performance claims.

Output as: Email N - Trigger - Subject - Body - CTA.

Before writing anything, ask me for these in one message and wait: (1) the product in one line, (2) the activation steps in order, ending with the event that defines an activated user, (3) funnel/event timing for each step or "unknown", (4) the biggest drop-off step and evidence of its blocker, if known.
```

## Real example output
Product: an analytics tool for product teams.
Activation steps: 1) connect a data source, 2) create first dashboard, 3) invite a teammate, 4) view a saved report after 7 days (activation event).
Known biggest drop-off: step 1 to 2 (connecting a source but never building a dashboard).

**Email 1 - Trigger: TEST WINDOW — no data source connected within 24 hours (validate against funnel data)**
Subject: your workspace is empty
Body: Right now there's nothing flowing into your workspace yet. Connect a source to start seeing your product data in one place.
CTA: Connect your first data source

**Email 2 - Trigger: TEST WINDOW — source connected, no dashboard within 48 hours (validate against funnel data)**
Subject: you've got data, now build with it
Body: Your data's flowing in. The fastest way to see it click is to build one dashboard around a single question you actually need answered this week, not a full reporting suite on day one.
CTA: Build your first dashboard

**Email 3 - Trigger: TEST WINDOW — dashboard created, no teammate invited within 5 days (validate against funnel data)**
Subject: dashboards are better shared
Body: Invite a teammate so the dashboard can support a shared conversation, not just one person's work.
CTA: Invite a teammate

## Why it works
Grounding each email in a real step from the actual funnel, not a generic "tips and tricks" cadence, means every email has one job: get the user unstuck at the exact point they're stuck. The drop-off flag puts the extra push where it statistically matters instead of spreading effort evenly across steps that don't need it.
