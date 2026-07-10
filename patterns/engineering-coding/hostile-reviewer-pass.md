---
title: "Hostile Reviewer Pass"
slug: "hostile-reviewer-pass"
format: "prompt"
category: "engineering-coding"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "beginner"
est_time: "10 min"
models: ["any"]
summary: "Make the agent tear apart its own diff like a hostile senior reviewer before you ship it."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["adversarial-fix-verification-loop", "skeptical-reader-gauntlet", "pre-push-merge-approval-gate"]
---

## When to use this
The change is written, tests pass, and the agent is telling you it's done. Run this before you commit or open a PR, while the diff is still cheap to fix. It works best on diffs big enough to hide something: 3+ files, or anything touching auth, money, or data writes.

## The pattern
```text
Review the current uncommitted diff as a hostile senior reviewer who did not write this code and gets no credit for being nice.

1. Run git diff and git diff --staged to get the full change. Review the diff plus enough surrounding code to judge it in context.
2. Hunt specifically for: broken edge cases, error paths that swallow failures, security holes (injection, missing authz, secrets in code), changes that contradict this codebase's existing conventions, and anything the diff changes but no test covers.
3. Report each finding with a severity:
   - BLOCKER: would cause a bug, data loss, or security issue in production.
   - SHOULD-FIX: works today but will bite someone later.
   - NIT: style or naming. Mention once, don't pad the list.
4. Fix every BLOCKER now, then re-run the affected tests and show the output. For SHOULD-FIX items, fix any that take under 10 minutes; list the rest for me to decide.
5. If you find zero BLOCKERs, do not just say "looks good." Name the three riskiest spots you checked and what convinced you each one is safe.
```

## Real example output
```
Reviewed diff: 4 files, +182/-31 (Stripe webhook handler for payment events).

BLOCKER (2):
1. routes/webhooks.ts:47: handler returns 200 before awaiting markInvoicePaid(). If that write fails, Stripe never retries and the invoice stays unpaid forever. Fixed: awaited the write, moved the 200 after it.
2. routes/webhooks.ts:12: signature check reads req.body after the JSON middleware already parsed it, so raw-body verification always fails open with the prod config. Fixed: mounted express.raw() on this route only.

SHOULD-FIX (1):
- No test for duplicate events (Stripe retries send the same event ID twice). Added one: second delivery is a no-op. Took 6 minutes.

NIT: handlePaymentIntentSucceeded is 80 lines and could split. Left alone.

Re-ran npm test webhooks after fixes: 11/11 passing.
```

## Why it works
An agent grading its own fresh work is the most generous reviewer alive. Recasting it as a hostile outsider flips the incentive: the persona gets credit for findings, not for finishing. The zero-BLOCKER clause closes the last loophole, because "here's what I checked and why it's safe" is work, while "looks good" is free.
