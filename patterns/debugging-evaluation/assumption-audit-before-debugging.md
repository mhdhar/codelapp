---
title: "Assumption Audit Before Debugging"
slug: "assumption-audit-before-debugging"
format: "goal"
category: "debugging-evaluation"
tools: ["universal"]
difficulty: "beginner"
est_time: "5 min"
models: ["any"]
summary: "List every assumption behind a bug theory and mark each confirmed, unconfirmed, or false."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You (or an AI agent) already have a theory about what's causing a bug and are about to start writing a fix. Before any code changes, you want to know how much of that theory is actually verified versus assumed.

## The pattern
```text
Bug: [BUG DESCRIPTION]
Current theory of the cause: [YOUR CURRENT THEORY, e.g. "the timeout is
too short for slow connections"]

Before we act on this theory, list every assumption it depends on, even
ones that seem obvious. For each assumption:

- State it as a single, checkable claim.
- Mark it CONFIRMED (and cite the exact evidence, e.g. a log line, a file
  and line number, a test result), UNCONFIRMED (nobody has checked yet),
  or FALSE (checked and it's wrong).
- If it's UNCONFIRMED, say exactly what check would confirm or reject it
  (a log to add, a query to run, a line of code to read) rather than
  leaving it vague.

Then answer directly: does the theory still hold if every UNCONFIRMED
assumption turns out false? If removing any one unconfirmed assumption
breaks the theory, say which one is load-bearing and check that one
first before writing any fix.
```

## Real example output
Bug: "Users on mobile report the upload feature times out."
Theory: "The timeout is too short for slow mobile connections."

Assumptions:
1. "Mobile users have slower upload speeds than desktop users on this feature." UNCONFIRMED. Check: pull upload duration by device type from the last 7 days of logs.
2. "The client-side timeout value is what's terminating the request, not a server-side limit." UNCONFIRMED. Check: read the actual error, is it a client `AbortError` or a server 504.
3. "The current timeout value is shorter than a typical successful mobile upload takes." UNCONFIRMED. Check: read the timeout constant in `uploadClient.ts` and compare against p95 upload duration.
4. "This is new behavior, not something that's always been true." CONFIRMED. Support tickets on this start exactly the week of the CDN migration, per the ticket timestamps already in this conversation.

Load-bearing check: assumption 2. If the actual error turns out to be a server 504 instead of a client-side abort, the entire "timeout is too short" theory is irrelevant, the fix would be in the wrong layer entirely.

Checked assumption 2: pulled 5 real error payloads from the mobile upload logs, all show `Error: Request aborted by client` with a 30000ms elapsed time matching the client's exact timeout constant. Assumption 2 is CONFIRMED. Proceeding to check assumption 3 (comparing that 30s constant against p95 mobile upload duration) before writing a fix.

## Why it works
A plausible-sounding theory can be built entirely on assumptions nobody checked, and fixing based on it wastes a cycle when the real cause turns out to live somewhere else. Naming which single assumption is load-bearing, and checking that one first, gets to "is this theory even in the right layer" in one step instead of after the fix already shipped.
