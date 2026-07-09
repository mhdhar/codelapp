---
title: "Macro Trend Relevance Scan for a Product Bet"
slug: "macro-trend-relevance-scan"
format: "goal"
category: "market-research"
tools: ["universal"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Filter broad macro trends down to the 2-3 that actually change your product bet."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You keep seeing macro trend reports (AI adoption, remote work, demographic shifts, regulatory change) and need to know which ones actually matter to your specific product bet, not a generic trends listicle.

## The pattern
```text
Act as a strategy analyst. My product bet and the broad trend area to
check are in the two lines at the bottom of this message.

Your goal: produce a short list of ONLY the trends within this broad area
that materially change whether this bet works, not a general trend overview.

A trend qualifies only if you can answer yes to: "if this trend reversed
tomorrow, would my bet get meaningfully worse?" If the answer is no, leave
it out even if it's a real and interesting trend.

For each qualifying trend, give:
- TREND: one line, specific, not generic ("enterprise SOC2 requirements are
  increasingly pushed down to vendors with under 20 employees," not "AI is
  growing")
- WHY IT MATTERS TO THIS BET: the direct causal link, not just topical
  relevance
- DIRECTION: accelerating, stalling, or reversing, and how confident you are
- SOURCED or ESTIMATE: tag whether this is grounded in a specific known
  data point/event, or is your inference
- TIME HORIZON: already priced into the market, or still early enough to be
  an edge

Stop when you have 2-4 trends that pass the "reverses tomorrow" test. If you
can't find at least 2 that pass, say so explicitly rather than padding the
list with topically-related-but-not-causal trends.

End with: which single trend, if it reversed, would hurt this bet the most,
and what you'd watch to catch that early.

My product bet:
Broad trend area to check (e.g. AI adoption, remote work, aging population, a regulatory shift):
```

## Real example output
```
Product bet: a tool that helps small agencies package and sell "AI
readiness audits" to SMB clients
Broad trend area: AI adoption among small businesses

TREND 1: SMB owners are being pitched AI tools directly by vendors but have
no internal capacity to evaluate what's actually useful versus hype
WHY IT MATTERS: this is the entire reason the audit product has a buyer, if
SMBs had internal AI literacy this service wouldn't be needed
DIRECTION: accelerating (ESTIMATE, inferred from the general proliferation
of AI tool marketing reaching SMB inboxes, no specific data point to cite)
TIME HORIZON: still early, AI hype coverage skews enterprise-focused, "how
do I evaluate this as an SMB" framing is not yet a crowded content space

TREND 2: general-purpose AI assistants are getting good enough at self-serve
"explain this to me" use cases that some SMB owners may just ask an AI
directly instead of paying a human auditor
WHY IT MATTERS: this directly competes with the paid audit, if self-serve AI
literacy tools get good enough, the addressable pain shrinks
DIRECTION: accelerating, genuinely uncertain how fast (ESTIMATE, this is a
real risk not a hypothetical, but the timeline is not something I can
confidently predict)
TIME HORIZON: uncertain, could erode the market in 12 months or 3 years,
this is the biggest unknown in the bet

DID NOT QUALIFY (considered and excluded): "AI is transforming enterprise
software" is a real trend but doesn't materially change this SMB-focused bet
either way, included here only to show it was considered, not missed.

BIGGEST RISK: Trend 2 reversing, i.e. self-serve AI tools getting good
enough that SMBs stop needing a human-led audit, would hurt this bet the
most. Watch for general-purpose AI assistants adding a built-in "audit my
business for AI opportunities" flow, that would be the leading indicator.
```

## Why it works
The "reverses tomorrow" test is a hard filter that kills topically-related-but-irrelevant trends before they pad the output. Naming a single leading indicator to watch turns a static trend scan into something you can actually monitor over time.
