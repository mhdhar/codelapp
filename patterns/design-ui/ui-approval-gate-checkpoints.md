---
title: "UI Approval Gate: Stop the Agent Before It Ships a Visual Change"
slug: "ui-approval-gate-checkpoints"
format: "workflow"
category: "design-ui"
tools: ["universal"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "Force your AI to describe the mockup and wait for your yes before touching production UI."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
---

## When to use this
You asked for a UI change and the agent went straight to editing files, three components deep, before you saw a single screenshot. Use this when you want a hard stop between "here's what I plan to build" and "I built it."

## The pattern
```text
Before you write or edit any UI code for this task, you must stop at these checkpoints and wait for my explicit approval at each one:

CHECKPOINT 1: Describe the visual change in plain language. List every screen, component, and state affected. Do not write code yet.

CHECKPOINT 2: Produce a text-based mockup or ASCII layout of the new UI (or a static HTML preview if the stack supports it). Show before and after for the affected area.

CHECKPOINT 3: List the specific files you will touch and what changes in each.

After each checkpoint, stop and ask "Approve to continue, or revise?" Do not proceed to the next checkpoint or write production code until I reply with an explicit yes.

Work out the affected pages and components yourself from my task description; that's
part of Checkpoint 1.

Task (the UI change I want):
```

## Real example output
```
CHECKPOINT 1: Visual change summary
- Pricing page: replace 3-column card grid with a comparison table on desktop, accordion on mobile.
- Affects: PricingPage.tsx, PricingTable.tsx (new), PricingCard.tsx (removed)
- States: default, "most popular" highlight row, mobile collapsed/expanded

Approve to continue, or revise?
```
User: approve
```
CHECKPOINT 2: Layout mockup

DESKTOP (>1024px)
+------------------+--------+--------+--------+
| Feature          | Basic  | Pro*   | Team   |
+------------------+--------+--------+--------+
| Users            |   1    |   5    |   20   |
| Support          |  Email | Priority| 24/7  |
+------------------+--------+--------+--------+
*Pro column has a highlighted top border and "Most Popular" badge.

MOBILE (<640px)
[ Basic  v ]  (collapsed accordion, tap to expand rows)
[ Pro*   v ]
[ Team   v ]

Approve to continue, or revise?
```

## Why it works
Naming the checkpoints forces the agent to externalize its plan before it commits to code, which is the only point where you can catch a wrong direction cheaply. Requiring an explicit reply, not just a pause, stops agents that treat silence or "looks fine" phrasing in their own output as permission to proceed.
