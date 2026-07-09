---
title: "Build-vs-Buy and Switching-Cost Analysis Against Incumbents"
slug: "build-vs-buy-switching-cost-analysis"
format: "workflow"
category: "market-research"
tools: ["universal"]
difficulty: "advanced"
est_time: "20 min"
models: ["any"]
summary: "Weigh building new against buying an incumbent, with real switching costs named, not assumed."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You're deciding whether to build a tool in-house or adopt an entrenched incumbent, or deciding whether your own product can realistically displace one, and need the switching-cost math made explicit instead of hand-waved.

## The pattern
```text
Act as a build-vs-buy analyst. Here's the situation:

WHAT WE'RE DECIDING: [BUILD IN-HOUSE VS BUY/ADOPT AN EXISTING TOOL, OR: OUR PRODUCT ENTERING A MARKET WITH THIS INCUMBENT]
INCUMBENT(S): [NAME THE DOMINANT EXISTING OPTION(S)]
WHO WOULD SWITCH/DECIDE: [WHO MAKES THIS CALL, E.G. "AN ENGINEERING TEAM ALREADY ON [INCUMBENT]"]

Do the following:

1. SWITCHING COST BREAKDOWN: list the real costs of moving away from the
   incumbent, split into:
   - HARD COSTS: migration effort, data export/import, retraining, contract
     termination penalties (name specifics if known, mark ESTIMATE if
     inferred)
   - SOFT COSTS: workflow disruption, loss of integrations, the political
     cost of admitting the old choice was wrong
   Rate the total switching cost LOW / MEDIUM / HIGH and say why.

2. INCUMBENT MOAT CHECK: name the specific things making the incumbent
   sticky beyond "they were first" (network effects, data lock-in,
   integration ecosystem, compliance certifications already done). If the
   moat is thin (just inertia, no real lock-in), say so plainly, don't
   assume every incumbent has a strong moat by default.

3. BUILD vs BUY vs DISPLACE comparison: rough effort and time to (a) build
   in-house, (b) adopt the incumbent as-is, (c) if applicable, what it would
   take for a challenger product to actually win a switch. Tag estimates as
   ESTIMATE.

4. THE REAL TRIGGER: name the one thing that would actually cause a switch
   away from the incumbent (a specific unmet need, a price increase, a
   reliability failure), not a generic "better UX" claim. Incumbents rarely
   lose to "we're nicer to use" alone, name what would have to be true.

5. RECOMMENDATION: given 1-4, say build, buy, or (if evaluating a challenge)
   enter-with-a-wedge or don't-enter, and name the single biggest risk to
   that recommendation.

Do not assume switching costs are low just because a new option looks
superficially better. Do not claim an incumbent's moat is strong or weak
without naming the specific mechanism behind that claim.
```

## Real example output
```
DECIDING: whether our internal tools team builds a custom feature-flagging
system or adopts LaunchDarkly
INCUMBENT: LaunchDarkly
WHO DECIDES: a 6-person platform engineering team on a rough in-house flag
table in Postgres today

SWITCHING COST BREAKDOWN (switching TO LaunchDarkly, away from the in-house
setup)
HARD COSTS: migrating ~40 existing flags and their targeting rules, roughly
2-3 engineer-days (ESTIMATE, based on typical flag migration scope, no
specific data for this team). LaunchDarkly seat pricing at ~15 engineers
likely lands $10-20K/year (ESTIMATE, inferred from published per-seat
tiers, exact contract pricing isn't public).
SOFT COSTS: the existing in-house flags are wired into internal dashboards
and alerting, that integration would need to be redone, probably a bigger
cost than the migration itself.
TOTAL SWITCHING COST: MEDIUM. Not hard lock-in (internal tool, no customer
data), but the dashboard/alerting rewiring is real, non-trivial engineering
time.

INCUMBENT MOAT CHECK: not applicable in the usual sense, since we're
evaluating buying LaunchDarkly, not displacing it. Its moat as a company is
real (mature targeting/experimentation features, SOC2 already done) but not
the relevant question for this specific build-vs-buy call.

BUILD vs BUY COMPARISON
- Build in-house properly (UI, audit log, percentage rollouts): ESTIMATE
  3-5 engineer-weeks for v1, plus an ongoing maintenance burden that's easy
  to underweight
- Buy LaunchDarkly: ESTIMATE 2-3 engineer-days integration, plus $10-20K/yr,
  but immediate access to experimentation and audit trail the team hasn't
  built

THE REAL TRIGGER: the actual reason to buy isn't "nicer UI," it's that the
team is already informally asking for percentage rollouts and an audit
trail, neither of which the in-house table has, and both would take real
engineering time to build well. If neither need existed, the switching cost
wouldn't be worth it for a tool that's already "good enough."

RECOMMENDATION: Buy. The real trigger (percentage rollouts + audit trail
already being requested) is present, and switching cost is medium, not
high, since this is internal tooling with no customer-facing lock-in.
Biggest risk to this recommendation: if actual flag count and complexity
stay low (under ~50 flags, no experimentation need), the in-house table
might remain "good enough" and $10-20K/year is a real cost to justify
against features nobody is actively blocked on.
```

## Why it works
Splitting hard costs from soft costs stops the soft costs (rewiring internal integrations, the political cost of reversing a past decision) from getting silently ignored, they're usually the bigger number. Requiring a real trigger, not "better UX," matches how incumbents actually get displaced: by a specific unmet need, not general polish.
