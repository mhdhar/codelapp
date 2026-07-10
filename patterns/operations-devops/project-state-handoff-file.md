---
title: "Project State Handoff File (Phase, Environment, Decisions)"
slug: "project-state-handoff-file"
format: "workflow"
category: "operations-devops"
tools: ["universal"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "Maintain one HANDOFF.md that survives across sessions: phase, ports, decisions log."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["session-handoff-note", "decision-log-append", "strategic-thread-tracker"]
---

## When to use this
You're mid-build on a project that spans many sessions, not just wrapping up one task. You need a single file that tracks build phase, what services are supposed to be running, and why past decisions were made, not just "where I stopped today."

## The pattern
```text
Update (or create) HANDOFF.md at the project root with the current project
state. This file persists across sessions, so overwrite stale sections but
don't delete the decisions log; append to it.

Write these exact sections:
1. One-liner: what this project is, in one sentence.
2. Current phase: where this build is right now (e.g. "Phase 6: backend
   wiring" or a plain description if you don't use numbered phases).
3. Last completed task: the most recent thing that was finished and
   verified working.
4. Next 3 tasks: specific enough to start on immediately, in priority order.
5. Known blockers: anything blocking progress: missing credentials,
   failing tests, an unresolved design question, an external dependency.
6. Environment state: every service currently expected to be running,
   its port, and any tunnel/preview URL (e.g. ngrok, localtunnel). Mark
   anything that needs to be started manually.
7. Decisions log: append one line per session: date, decision made, and
   the one-sentence reason. Do not rewrite past entries.

Project root: the root of the repo we're working in (where .git lives),
unless I name a different directory in my next message. If HANDOFF.md
already exists there, read it first, carry forward anything still true,
and only update what's changed.
```

## Real example output
```
HANDOFF.md updated at ~/projects/wataseat/HANDOFF.md

One-liner: WataSeat is a booking app for charter boat captains to manage
seat availability and conversational trip requests.

Current phase: Phase 6 - backend wiring (Stripe checkout + Supabase booking table)

Last completed task: Seat availability calendar renders and reads from
Supabase; verified against 3 test captains' accounts.

Next 3 tasks:
1. Wire Stripe checkout session creation to the booking confirm button.
2. Add webhook handler for checkout.session.completed -> mark booking paid.
3. Add captain-side cancellation flow with refund window logic.

Known blockers: Stripe account is in test mode only, live keys blocked on
business verification (pending, ETA unknown).

Environment state:
- Next.js dev server: port 3000, not currently running, start with `npm run dev`
- Supabase local: port 54321, running
- ngrok tunnel for Stripe webhook testing: not active, run
  `ngrok http 3000` and update the webhook URL in the Stripe dashboard
  when resumed

Decisions log:
- 2026-07-02: Chose Supabase over Firebase for bookings: needed relational
  joins between captains, boats, and trips that Firestore made awkward.
- 2026-07-09: Deferred live Stripe keys until business verification clears;
  building the full flow against test mode in the meantime.
```

## Why it works
A per-task handoff note answers "where did I stop." This file answers "what is true about this project right now", including the environment state that a fresh session can't infer from git alone, like which port a tunnel URL points at. The decisions log stops the same debate from being re-litigated every few sessions.
