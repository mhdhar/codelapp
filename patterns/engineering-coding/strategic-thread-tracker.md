---
title: "Strategic Thread Tracker"
slug: "strategic-thread-tracker"
format: "loop"
category: "engineering-coding"
tools: ["universal"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "Open every session by placing today's task inside the bigger roadmap, not as an isolated request."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're picking up a project that spans many sessions, and the risk isn't "can I do this task" but "is this task the right one right now." Use it at the start of any session on a project you've touched before, especially after a gap of days.

## The pattern
```text
Before doing any work this session, orient inside the project's longer arc.

1. Establish where the project actually stands: inspect the repo (recent commits, README, open TODOs, any handoff notes) and summarize the current state in one paragraph. If I give you a last-known-state note, start from that and verify it against the repo.
2. Restate the project goal and place today's request on the map: is it (a) on the critical path toward the goal, (b) a useful but non-critical detour, or (c) unrelated / should be deferred?
3. If it's (b) or (c), say so plainly and ask whether I want to proceed anyway, before writing any code.
4. List up to 3 things that should explicitly wait until later, and 1 thing that should be avoided entirely given the current state (e.g. a change that would conflict with unfinished work elsewhere).
5. Only after this orientation, proceed with today's request.

First ask me, in one message, for the project goal and today's request, plus a last-known-state note if I have one. Wait for my reply before touching the repo.
```

## Real example output
```
1. Repo check: last commit 4 days ago, "Add Stripe webhook handler". README still lists auth as "in progress". No handoff note found.

2. Project goal: ship a subscription-gated newsletter tool by end of quarter.
Today's request: "add a dark mode toggle to the settings page."
Placement: (b) useful but non-critical detour. Auth is still incomplete and blocks the subscription flow, which is the actual critical path.

3. Dark mode is a 20-minute add with no dependency risk, so I can do it, but flagging: auth has been "in progress" for 4 days with no visible progress. Want me to do dark mode first, or pivot back to auth?
   > Finish auth first, dark mode can wait.

4. Waiting: dark mode toggle, Stripe webhook retry handling, settings page polish.
   Avoid: don't touch the settings page layout right now — the auth work will add a "verify email" banner there, and a layout change now would just get overwritten.

Proceeding with auth instead of the original request, per your answer.
```

## Why it works
Individual sessions optimize for "finish what's in front of me," which quietly drifts a multi-week project sideways. Forcing a one-paragraph roadmap check before touching code catches the drift while it's still a one-line correction, not a week of wasted work.
