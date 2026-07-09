---
title: "Rewrite One Draft for a Different Audience or Register"
slug: "audience-register-rewrite"
format: "prompt"
category: "content-writing"
tools: ["universal"]
difficulty: "beginner"
est_time: "5 min"
models: ["any"]
summary: "Convert an exec summary into a technical deep-dive (or the reverse) without rewriting from scratch."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You already wrote a piece for one audience, engineers, executives, or customers, and now need the same substance for a different reader. Starting over loses the parts of the first draft that already work.

## The pattern
```text
Rewrite the text I paste below for a different audience. Keep every
factual claim and conclusion identical, only change how it's explained.

Infer the original audience from the text itself and state it in one
line before the rewrite. The new audience is named at the bottom.

Rules:
- If moving from technical to non-technical: replace jargon with what it
  does, not a dumbed-down version of what it is. Cut implementation
  detail the new audience doesn't need to act on this.
- If moving from non-technical to technical: add the specifics a
  technical reader needs to trust the claim (numbers, mechanism, method),
  don't just add jargon for tone.
- Keep the same conclusion and recommendation. Don't soften a clear
  recommendation into a vague one to seem safer for the new audience.
- Length can change to fit the new audience's attention span. Note the
  original and new word count at the end.

New audience (e.g. "executives with no technical background"):

Paste the text below this line:
```

## Real example output
Original (engineers): "We moved rate limiting from the app layer to the edge with a token bucket algorithm, capped at 100 req/s per API key with a 20-request burst allowance. This cut p99 latency on the auth endpoint from 340ms to 90ms because we stopped rejecting requests after they'd already paid the cost of hitting our servers."

Rewrite (executives): "We moved traffic filtering to happen before requests reach our servers instead of after. That change alone cut the slowest login response times by nearly 4x, because we're no longer doing expensive work on requests we were going to reject anyway." (62 words → 41 words)

## Why it works
Naming the specific failure mode for each direction, jargon-for-jargon's-sake going technical, over-simplification going non-technical, stops the generic "make this more accessible" instruction from either flattening the substance or leaving it unreadable. Locking the conclusion in place prevents the common drift where a softened rewrite also quietly softens the actual recommendation.
