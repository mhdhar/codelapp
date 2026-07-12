---
title: "Batch Every Clarifying Question Before Planning Starts"
slug: "clarifying-questions-batch"
format: "workflow"
category: "product-planning"
tools: ["universal"]
difficulty: "beginner"
est_time: "10 min"
models: ["any"]
summary: "Batch every clarifying question into one list instead of guessing or asking one at a time."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["ambiguity-halt-and-ask", "pre-build-intent-capture", "decision-batch-checkpoint"]
---

## When to use this
You've given an agent a request with real gaps in it (unclear scope, unclear users, unclear constraints), and you'd rather answer everything once than get interrupted five separate times, or worse, have it silently guess and build the wrong thing.

## The pattern
```text
I have a request for you, but don't start implementation yet. When I give it to you, first inspect the request and any already-supplied instructions, docs, or relevant code read-only. Then list only the material questions that inspection cannot answer, grouped into:
- Scope: what's in/out
- Users: who this is for, who it explicitly isn't for
- Constraints: technical, timeline, budget
- Success definition: how we'll know this worked

Only include questions you actually need answered, not ones you could reasonably assume. If a section has no real gaps, write "none" under it. If every section is "none," say "No clarifying questions; proceeding with the supplied request," state any defaults, and proceed without waiting. Otherwise stop after the list and wait for my answer.

Once I've answered, proceed with the original request using my answers as fixed constraints. Those answers define scope; they do not authorize external or irreversible actions. If you hit a new ambiguity mid-work that materially changes scope, stop and ask rather than assuming; everything else you can decide yourself.

Start by asking me to paste the request, then wait for it.
```

## Real example output
"SCOPE: Does the expense tool need multi-currency support at launch, or USD only?
USERS: none; target user (US freelancers) is already clear from the brief.
CONSTRAINTS: Any existing tax categories to map to (IRS Schedule C categories, or custom)? Hard deadline, or scoped to 'whenever it's solid'?
SUCCESS DEFINITION: Is success 'user manually confirms categorization is right' or 'categorization accuracy hits X% without user correction'?

(after answers) Proceeding: USD only, IRS Schedule C categories, no hard deadline, success = 90%+ auto-categorization accuracy without correction."

## Why it works
A single batched list costs you one interruption instead of five staggered ones, and forces the agent to distinguish "actually blocking" ambiguity from things it could reasonably infer, so you're not fielding lazy questions it should have answered itself.
