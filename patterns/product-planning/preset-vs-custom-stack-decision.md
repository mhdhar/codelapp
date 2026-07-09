---
title: "Known-Good Preset vs. Custom Architecture Decision"
slug: "preset-vs-custom-stack-decision"
format: "goal"
category: "product-planning"
tools: ["universal"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Weigh a known-good stack preset against custom architecture with a forced, specific justification."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're scoping a new build and reflexively reaching for a from-scratch stack, or reflexively reaching for a boilerplate, without actually comparing either one against what this specific project needs.

## The pattern
```text
I'm scoping [PROJECT DESCRIPTION]. Decide whether to use a known-good preset/boilerplate or build custom architecture.

1. List the preset(s) that plausibly fit this project's shape (name real ones you know, e.g., a SaaS starter kit, a CMS-plus-storefront combo, a chat-app scaffold — whatever matches [PROJECT DESCRIPTION]).
2. For the closest-fitting preset, list exactly what it gives you for free and what constraints it imposes (opinionated folder structure, locked-in auth provider, specific database, etc.).
3. List the specific requirements in [PROJECT DESCRIPTION] that the preset can't satisfy, if any.
4. Recommend preset or custom. If custom, the reason must name a concrete requirement the preset blocks, not a vague preference like "more control" or "cleaner code."
5. If preset: name the exact scaffold command. If custom: list the 3-5 architecture decisions I still need to make before writing code.

Target timeline: [ROUGH TIMELINE, e.g., "ship in 2 weeks" or "no deadline"].
```

## Real example output
"1. Closest fit: an Open SaaS-style Wasp starter — bundles auth, Stripe, admin dashboard, landing page.
2. Free: email/Google/GitHub auth, Stripe + Polar + Lemon Squeezy payment rails, S3 uploads, AI-agent-readable project docs pre-wired. Constraints: locked into the starter's config DSL and Prisma, opinionated folder layout, harder to eject later.
3. Requirement the preset can't satisfy: the project needs a real-time collaborative canvas (WebSocket-driven), which the starter has no primitive for.
4. Recommendation: preset for everything except the canvas. Use the starter for auth/payments/dashboard, bolt on a custom WebSocket service for the canvas feature. Full custom would mean rebuilding auth and Stripe integration the preset gives for free, for no benefit tied to the actual blocking requirement.
5. Scaffold: initialize the SaaS starter, then add a standalone `canvas-service/` with its own deploy target.
Timeline: 2 weeks — the preset saves roughly 10 days on auth and billing, leaving the full runway for the canvas feature."

## Why it works
Forcing the model to name what a preset gives away for free and what it locks you into turns "should I use a boilerplate" from a vibe into a checklist. Requiring the custom-build justification to name a specific blocked requirement kills the most common failure mode: building custom because it feels more "real," not because the preset actually can't do the job.
