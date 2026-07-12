---
title: "Pre-Registered Demand Test: Commit the Pass/Fail Numbers First"
slug: "pre-registered-demand-test-design"
format: "goal"
category: "market-research"
tools: ["universal"]
difficulty: "beginner"
est_time: "15 min"
models: ["any"]
summary: "Design a one-week demand test where the kill threshold is written down before you see a single result."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["demand-signal-validation-check", "riskiest-assumption-kill-loop", "ab-test-ship-kill-verdict"]
---

## When to use this
You're about to run a landing page, waitlist, or fake-door test for a product idea, and you know from experience that once the numbers come in, you'll rationalize whatever they say. You want the pass/fail line committed before launch.

## The pattern
```text
Act as an experiment designer.

Your goal: a one-page demand test plan I can run in 7 days or less, with
pass/fail thresholds committed BEFORE launch. The plan is done when every
item below is filled in with a specific number or a specific artifact,
no "it depends" left anywhere.

1. TEST FORMAT: pick ONE cheapest-sufficient format for this idea (landing
   page + waitlist, fake-door button in an existing product, concierge
   offer in a community, pre-order link). Say why the others are overkill
   or under-powered for this specific idea.

2. THE COSTLY ACTION: define what a visitor must do for it to count as
   demand. An email address is weak, pick the most costly action this
   format allows (a card-required preorder, a booked call, a filled intake
   form) and say what each level of costliness would prove.

3. PRE-REGISTERED THRESHOLDS: given the audience size I can reach, commit
   three numbers now:
   - KILL: below this conversion, I stop and don't relitigate
   - CONTINUE: above this, I run the next bigger test
   - ZONE OF SELF-DECEPTION: the range in between, and what single
     follow-up question decides it (e.g. 5 replies to a "why did you sign
     up" email)
   Base these on a supplied historical benchmark or a cited conversion norm for the chosen format, and say
   which one you're using. If neither is available, label the threshold a provisional assumption rather than evidence. If I could talk myself into calling any result
   a win, the thresholds are too soft, tighten them.

4. CONTAMINATION LIST: 3 ways this test could false-positive (friends
   clicking, a misleading headline overselling, traffic from people who
   can't buy) and one concrete guard for each.

5. THE COPY: draft the actual headline, subhead, and call-to-action button
   text for the test. Plain claims only, nothing the real product wouldn't
   deliver, an oversold test validates the wrong product.

End with the plan as a checklist I can execute Monday morning, in order,
with the three committed numbers at the top.

Start by asking me in one message for: (1) my product idea, (2) the
audience I can realistically reach this week, channel and rough size, and
(3) any historical conversion benchmark or source I trust. If the reachable
audience cannot support a meaningful test in seven days, say so instead of
inventing decisive thresholds. Wait for my reply before designing anything.
```

## Real example output
```
COMMITTED NUMBERS (audience: ~2,000 newsletter readers, indie Shopify store
owners)
KILL: under 1.5% click-to-intake (fewer than 8 completed intake forms per
~550 expected visitors). Stop. Do not rerun with "better copy."
CONTINUE: 4%+ completed intake forms (22+). Run a $200 paid-traffic
replication next.
SELF-DECEPTION ZONE: 1.5-4%. Decider: email all signups "what would you
expect this to cost per month?" - 5+ replies naming a number = continue,
mostly silence = kill.

TEST: landing page + intake form (not just email: form asks store URL and
current monthly returns volume, a 2-minute cost that filters tourists).

CONTAMINATION GUARDS: no personal-network shares until day 3; headline says
"reduce return fraud" not "eliminate returns"; intake rejects stores under
50 orders/month since they can't buy at the planned price.

COPY: headline "Catch serial returners before you refund them." CTA:
"Check my store's exposure" (not "Join waitlist").

MONDAY CHECKLIST: 1. page live, 2. intake wired to sheet, 3. thresholds
pinned in team channel BEFORE the newsletter goes out, 4. send, 5. tally
Friday against the pinned numbers.
```

## Why it works
Committing kill/continue numbers before seeing data is the same mechanism as pre-registration in research: it removes the option to move the goalposts after the fact. Naming the self-deception zone and its tiebreaker question in advance handles the most common real outcome, an ambiguous middle result, without a second round of motivated reasoning.
