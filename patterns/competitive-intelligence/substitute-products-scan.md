---
title: "Substitute Products Scan"
slug: "substitute-products-scan"
format: "prompt"
category: "competitive-intelligence"
tools: ["universal"]
difficulty: "beginner"
est_time: "8 min"
models: ["any"]
summary: "Find who else solves this problem, including spreadsheets and manual workarounds, not just named rivals."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
Before a competitive brief or market-sizing conversation that only lists direct, named competitors — because the biggest source of lost deals is often "we just do it in a spreadsheet," not a rival product.

## The pattern
```text
The problem my product solves and the target buyer are at the bottom of this prompt.

List every distinct way this buyer could be solving this problem today, in four tiers:
1. Direct competitors: other named products built specifically to solve this problem
2. Adjacent tools: products built for a different primary purpose that get repurposed to solve this problem (e.g. a spreadsheet, a project management tool, a generic form builder)
3. Manual workarounds: solving it with people and process instead of software (a spreadsheet template, a shared inbox, a recurring meeting, hiring someone to do it by hand)
4. Do-nothing: what happens if the buyer just doesn't solve this problem at all — is that a real option for them, or does it cause a visible failure?

For each tier, name the specific substitute and, in one line, why a buyer might genuinely prefer it over buying a dedicated product like mine (cost, simplicity, "we're too small for this," trust, existing habit). Do not dismiss any tier as not real competition — the goal is to see the full field, including the substitutes that don't look like competitors.

End with which substitute is most likely my actual biggest competitor for this buyer, and why it's probably not the direct competitor I'd have named first.

The problem my product solves:
Target buyer:
```

## Real example output
1. Direct competitors: two or three named products with the same core feature set, differing mainly on price and integration depth.
2. Adjacent tools: a generic spreadsheet tool used with a shared template, preferred by very small teams because it's free and everyone already knows how to use it. A general project management tool with a custom board, preferred by teams who don't want another login.
3. Manual workarounds: a shared inbox plus a recurring weekly sync meeting, preferred by teams who don't yet believe the problem is big enough to justify buying software for it. A single ops person doing it by hand, common at small companies where headcount is cheaper than a new tool budget line.
4. Do-nothing: skipping the problem entirely is a real option below a certain team size; the failure it causes (missed handoffs, duplicated work) is often invisible until the team is bigger.

Most likely actual biggest competitor: the spreadsheet-plus-shared-template workaround, not the named direct competitors. It's free, requires no buying process, and most buyers only leave it once the pain becomes visible and specific enough to justify software — meaning the real sales objection is usually "we don't need a tool for this yet," not "we prefer a rival product."

## Why it works
Forcing all four tiers, including "do-nothing," stops the scan from defaulting to the two or three names everyone already thinks of. Naming why a buyer would genuinely prefer each substitute, not just that it exists, turns this into something sales can actually use to handle the real objection instead of the assumed one.
