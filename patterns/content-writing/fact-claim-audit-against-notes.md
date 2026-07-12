---
title: "Audit a Draft's Claims Against Your Source Notes"
slug: "fact-claim-audit-against-notes"
format: "goal"
category: "content-writing"
tools: ["universal"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Catch every fact, number, or quote a draft invented that isn't actually in your source material."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["readme-truth-audit", "concrete-example-injector", "battlecard-rot-check"]
---

## When to use this
An AI, or a fast-moving collaborator, wrote a draft based on your notes, an interview, or your data, and you're about to publish it. You need to know exactly which claims are backed by your source material and which got quietly invented or exaggerated, before a reader finds them first.

## The pattern
```text
Goal: every factual claim, number, quote, and named example in my
draft is either traceable to the source notes or flagged. Zero
unflagged unsourced claims when you're done.

Process:
1. List every specific, checkable claim in the draft: statistics, dates,
   quotes, named people or companies, cause-and-effect statements
   ("X caused Y"), and comparisons ("more than," "the first").
2. For each one, search the source notes for direct support. Mark it:
   - SOURCED: quote the exact line in the notes that supports it
   - SOFTENED: the notes support a weaker version of this claim than the
     draft states, show both versions
   - NOT SUPPORTED BY THE SUPPLIED SOURCES: the supplied notes do not support
     this claim; that alone does not establish that it is false or who wrote it
3. For every SOFTENED or NOT SUPPORTED BY THE SUPPLIED SOURCES claim, rewrite the draft sentence to
   match what the notes actually support, or mark it (VERIFY: source) if
   you genuinely don't know and I need to check it myself.
4. Output the corrected draft first, then the full claim list from step 2
   underneath it so I can see your work.

Before you do anything, ask me in one message for both: (1) the source
notes, interview, or data, (2) the draft to audit. Then wait until you
have both.
```

## Real example output
- "Support tickets dropped 40% after the change" - NOT SUPPORTED BY THE SUPPLIED SOURCES. Notes say "tickets went down noticeably" with no number. Rewrote to: "Support tickets dropped noticeably after the change."
- "This is the same approach Stripe uses internally" - NOT SUPPORTED BY THE SUPPLIED SOURCES. Not mentioned anywhere in the notes. Flagged (VERIFY: source) and removed the sentence pending a check.
- "Three of our five biggest customers asked for this" - SOURCED. Notes: "Acme, Bolt, and Fenwick all filed this as a request last quarter."

## Why it works
Splitting into SOURCED / SOFTENED / UNSOURCED instead of a binary true-or-false check catches the more common failure mode: claims that aren't fabricated outright but got quietly strengthened past what the source actually says. Naming the claim types up front (numbers, quotes, comparisons) stops the model from skimming past a soft claim like "the first company to do this."
