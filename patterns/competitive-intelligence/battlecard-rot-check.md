---
title: "Battlecard Rot Check"
slug: "battlecard-rot-check"
format: "loop"
category: "competitive-intelligence"
tools: ["universal"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Re-verify every claim in your sales battlecard against competitors' live pages and flag what went stale."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["competitor-watch-loop", "competitor-messaging-audit", "readme-truth-audit"]
---

## When to use this
Your competitor battlecard was accurate the day it was written. Competitors reprice, ship the feature you said they lacked, and rewrite their homepage, and the battlecard doesn't know. Run this monthly, or before any deal cycle big enough that a rep quoting a wrong price would hurt.

## The pattern
```text
This is a recurring audit of my sales battlecard.

1. Extract every factual claim in the battlecard that could go stale: competitor prices, tier names, feature gaps ("they don't have X"), limits, integrations, quoted positioning. Number them. Skip claims about my own product.
2. For each numbered claim, visit the competitor page that would prove or disprove it (pricing, features, changelog, docs). Check the claim against what the page says today.
3. Mark each claim with exactly one verdict:
   - VERIFIED: the page still supports it. Cite the URL.
   - STALE: the page contradicts it. Quote the exact line that contradicts it and write the corrected claim.
   - UNVERIFIABLE: nothing public confirms or denies it. Say what would be needed to check it.
4. Loop until every numbered claim has a verdict. Do not skip claims that look obviously fine. Pricing claims that look obviously fine are the ones that rot first.

Output the battlecard with each claim annotated inline, then one summary line: X verified / Y stale / Z unverifiable. If more than a third of claims are stale, say so bluntly at the top. A battlecard that is a third wrong is worse than none, because reps trust it.

Start by asking me to paste the battlecard, then wait. Don't start until you have it.
```

## Real example output
14 claims extracted. Summary: 9 verified / 4 stale / 1 unverifiable.

Stale claims:
- Claim 3: "No SSO below Enterprise." Their pricing page now says "SAML SSO included" on the $79 Team tier. Corrected: "SSO available from Team tier. Our edge is now SCIM provisioning, still Enterprise-only for them."
- Claim 7: "Starts at $49/mo." Pricing page shows $59/mo as the entry price. Corrected accordingly.
- Claim 9: "No public API rate-limit docs." They published /docs/rate-limits in May. Claim removed.
- Claim 12: The quoted homepage tagline no longer appears anywhere on their site. Replaced with the current headline.

Unverifiable: Claim 11: "Their support response time averages 48 hours." Nothing public. Needs a win-loss quote or a trial-account test to keep.

## Why it works
A battlecard is just a list of checkable claims, and forcing one of three verdicts on every numbered claim stops the model from skimming and blessing the whole document. Quoting the contradicting line makes each stale flag auditable before you edit anything, so the fix round takes minutes.
