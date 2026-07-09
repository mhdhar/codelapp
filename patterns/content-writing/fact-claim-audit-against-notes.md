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
---

## When to use this
An AI, or a fast-moving collaborator, wrote a draft based on your notes, an interview, or your data, and you're about to publish it. You need to know exactly which claims are backed by your source material and which got quietly invented or exaggerated, before a reader finds them first.

## The pattern
```text
Goal: every factual claim, number, quote, and named example in the
draft I paste below is either traceable to the source notes or flagged.
Zero unflagged unsourced claims when you're done.

Process:
1. List every specific, checkable claim in the draft: statistics, dates,
   quotes, named people or companies, cause-and-effect statements
   ("X caused Y"), and comparisons ("more than," "the first").
2. For each one, search the source notes for direct support. Mark it:
   - SOURCED: quote the exact line in the notes that supports it
   - SOFTENED: the notes support a weaker version of this claim than the
     draft states, show both versions
   - UNSOURCED: nothing in the notes supports this, it was invented
3. For every SOFTENED or UNSOURCED claim, rewrite the draft sentence to
   match what the notes actually support, or mark it (VERIFY: source) if
   you genuinely don't know and I need to check it myself.
4. Output the corrected draft first, then the full claim list from step 2
   underneath it so I can see your work.

Paste the notes, interview, or data after SOURCE NOTES and the draft
to audit after DRAFT.

SOURCE NOTES:

DRAFT:
```

## Real example output
- "Support tickets dropped 40% after the change" — UNSOURCED. Notes say "tickets went down noticeably" with no number. Rewrote to: "Support tickets dropped noticeably after the change."
- "This is the same approach Stripe uses internally" — UNSOURCED. Not mentioned anywhere in the notes. Flagged (VERIFY: source) and removed the sentence pending a check.
- "Three of our five biggest customers asked for this" — SOURCED. Notes: "Acme, Bolt, and Fenwick all filed this as a request last quarter."

## Why it works
Splitting into SOURCED / SOFTENED / UNSOURCED instead of a binary true-or-false check catches the more common failure mode: claims that aren't fabricated outright but got quietly strengthened past what the source actually says. Naming the claim types up front (numbers, quotes, comparisons) stops the model from skimming past a soft claim like "the first company to do this."
