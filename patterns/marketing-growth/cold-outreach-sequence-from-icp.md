---
title: "Generate a Cold Outreach Sequence From a Real ICP and Pain Point"
slug: "cold-outreach-sequence-from-icp"
format: "prompt"
category: "marketing-growth"
tools: ["universal"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Turn one specific ICP and a sourced pain point into a 3-email sequence that isn't generic spam."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["target-user-segment-breakdown", "competitor-complaint-mining", "onboarding-sequence-from-activation-steps"]
---

## When to use this
You've identified a specific ICP segment and a real, sourced pain point (from interviews, support patterns, or lost-deal notes) and need a first-touch outreach sequence for it. Not for "write me a cold email template," which produces the same spam everyone already deletes.

## The pattern
```text
Write a 3-email cold outreach sequence from the ICP, pain point, and offer I give you.

Structure:
Email 1 (Day 0): Open with an observation about their specific situation, not "hope this finds you well." End with one low-friction ask: a question they can answer in one line, not a meeting request.
Email 2 (Day 3, if no reply): Lead with the proof point. If I said "no proof yet," write the line NEED PROOF POINT instead of inventing a number or client name. Lower the ask further: a yes/no question.
Email 3 (Day 7, if no reply): A breakup email. Give them permission to say not now, leave the door open, no guilt-tripping.

Rules:
- No flattery ("I love what you're doing"), no fake urgency, no "just circling back."
- Each email under 120 words.
- Subject line under 6 words, no clickbait.
- If the pain point I gave you is too generic to write from ("companies struggle with growth"), tell me instead of writing generic copy anyway.

Start by asking me for these in one message, then wait: (1) the ICP (role, company size/type, the tool or process they use today), (2) the pain point, something I actually observed or heard, not a guess, (3) our offer in one sentence, plus a proof point with a real number, or "no proof yet" if there isn't one.
```

## Real example output
ICP: Head of Support at 50-200 person B2B SaaS companies running Zendesk.
Pain point: ticket volume spikes 3-4x during release weeks and they have no way to predict staffing needs ahead of time, so they're either overstaffed most weeks or scrambling during releases.
Proof: one team cut release-week overtime hours by 60% using our release-calendar integration.

**Email 1, Day 0**
Subject: release week staffing

Saw your last two release notes went out on a Thursday. That's usually when support queues spike hardest for teams on a two-week sprint cycle.

Quick question: do you staff release weeks differently than normal weeks right now, or is it the same rotation every time?

**Email 2, Day 3**
Subject: re: release week staffing

One support team we work with was scheduling overtime for every release until they connected their release calendar to their staffing plan. That cut release-week overtime hours by 60% in the first quarter.

Worth 10 minutes to see if the same setup would work for your rotation?

**Email 3, Day 7**
Subject: closing the loop

Haven't heard back, so I'll assume release-week staffing isn't a priority right now. Totally fine. If that changes, I'm easy to find. Good luck with the next release.

## Why it works
Requiring a sourced pain point up front, and forcing the model to flag a generic one instead of writing around it, is what keeps this out of spam territory. The declining-ask structure (question, then yes/no, then permission to close) matches how real replies happen instead of pushing a meeting from email one.
