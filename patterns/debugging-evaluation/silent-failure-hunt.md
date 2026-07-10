---
title: "Silent Failure Hunt"
slug: "silent-failure-hunt"
format: "goal"
category: "debugging-evaluation"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "intermediate"
est_time: "15 min"
models: ["any"]
summary: "Scan a codebase area for swallowed errors and ignored rejections that hide real bugs."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["error-message-forensics-loop", "bug-postmortem-from-fix", "component-states-completeness-check"]
---

## When to use this
You suspect bugs are getting silently eaten somewhere (a user reports something "just didn't work" with no error, no log line, nothing) and want a targeted sweep for swallowed errors before you go bug-hunting blind in a specific feature.

## The pattern
```text
Scan this repo's application source for places where an error or failure
is being silently swallowed instead of surfaced. Skip tests, vendored
code, and generated files. If I name a narrower directory in my next
message, scan only that. Look specifically for:

- catch blocks that don't log, rethrow, or otherwise surface the error
  (empty catch, or a catch that only does something unrelated)
- promises/async calls with no `.catch()` and not inside a try/await,
  where a rejection would vanish
- functions that return `null`/`false`/`undefined` on failure with no
  way for the caller to distinguish "failure" from a legitimate empty
  result
- error handlers that log at a level nobody monitors (e.g. `console.log`
  instead of the real logger) or log a generic message that drops the
  original error object/stack

For each one found, output a row with:
- File:line
- What's being swallowed (quote the actual code)
- Why it's risky (what real failure would go unnoticed, be specific to
  what this code does, not generic)
- Suggested fix (log with context, rethrow, or return a typed
  result/error, whichever fits the surrounding code's existing error
  handling style)

Rank the list by risk, highest first: something in a payment, auth, or
data-write path outranks something in a purely cosmetic UI path. Do not
flag defensive code that intentionally and explicitly ignores a specific,
known-safe error (e.g. an `ENOENT` check with a comment) as a problem.
```

## Real example output
| File:line | Swallowed | Risk | Suggested fix |
|---|---|---|---|
| `src/services/webhooks.ts:41` | `catch (e) {}` around `await notifyPartner(order)` | A failed partner notification after a paid order silently never happens, no retry, no alert, partner never told the order shipped | Log the error with order ID and rethrow into the existing job-failure queue so it retries |
| `src/lib/inventory.ts:88` | `catch (e) { console.log('inventory error') }` around a stock decrement | Stock count can silently fail to decrement on a real DB error, causing overselling, and `console.log` isn't shipped to any monitored logger in production | Replace with `logger.error('inventory.decrement failed', { sku, error: e })` and rethrow so the order fails instead of appearing to succeed |
| `src/utils/parseAddress.ts:19` | Returns `null` on any parse failure, same as a legitimately empty input | Caller in `checkout.tsx:102` treats `null` as "no address entered" and shows a generic validation message, hiding real parse bugs (e.g. malformed ZIP) from both the user and any error tracking | Return a discriminated result (`{ ok: false, reason }`) instead of `null` so callers can show a specific error and it can be logged |

Ranked: `webhooks.ts:41` first (payment/fulfillment path), `inventory.ts:88` second (data-write path), `parseAddress.ts:19` last (UX-only impact).

## Why it works
Swallowed errors don't show up in bug reports because there's no error to report, someone just notices a feature "quietly doesn't work" weeks later. A scoped, criteria-based sweep finds these before a user does, and ranking by which code path they sit in (payment/auth/data-write vs. cosmetic) tells you which ones to fix today versus file for later.
