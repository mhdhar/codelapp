---
title: "Error Message Forensics Loop"
slug: "error-message-forensics-loop"
format: "loop"
category: "debugging-evaluation"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "Force the AI to parse the exact error text and stack trace before it's allowed to guess a cause."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["root-cause-debugging-loop", "log-diff-divergence-loop", "silent-failure-hunt"]
---

## When to use this
You have a real error message or stack trace in hand and the AI's first move is "this is probably a [library] issue" before it has read a single line of the trace. You want the trace read literally, not pattern-matched against a generic cause.

## The pattern
```text
Work from the exact error output, not from what this error type
usually means.

Before proposing any cause, do this in order and show your work at each step:

1. Quote the exact error message text back to me, word for word. Do not
   paraphrase or summarize it.
2. List every file:line reference in the stack trace, in the order the
   trace gives them, from where the error was thrown down to where it
   originated in application code. Skip frames only if the trace itself
   marks them as library/framework internals.
3. Open and read each of those file:line locations, starting from the
   deepest application-code frame. Quote the actual code at that exact
   line, not a nearby line or the general function.
4. State what the error literally means at that specific line: trace the
   variable, argument, or condition that has to be undefined, wrong-typed,
   or false for this exact error to fire there. Do not describe what this
   error type "usually means" in general.
5. Only after step 4, state the root cause as one sentence tied to the
   specific line and value you just traced.

If a frame has no resolvable file:line, say so explicitly instead of
guessing which file it might be. Do not name a library, framework, or
"common cause" for this error type until steps 1-4 are complete on the
actual trace.

First, ask me to paste the full error / stack trace, unedited, then
wait. If I give a file path or a command instead, read or run it
yourself to get the raw output.
```

## Real example output
Step 1: "TypeError: Cannot read properties of undefined (reading 'id')"

Step 2: Frames in application code: `getUserOrders (src/services/orders.ts:34:19)`, called from `OrdersPage (src/pages/orders.tsx:19:26)`.

Step 3: `orders.ts:34` reads `const orderId = session.user.id;`. `orders.tsx:19` calls `getUserOrders(session)` inside `useEffect`, before the auth provider's `session` has resolved from `undefined` to the real object.

Step 4: On the first render, `session` is `undefined` by design (the auth hook initializes it that way until the token check resolves), so `session.user` throws before `.id` is ever reached. This isn't a library bug, it's a call made one render too early.

Step 5: Root cause: `OrdersPage` calls `getUserOrders(session)` before checking `session` is loaded, so `session.user.id` runs against `undefined`.

Fix: added `if (!session) return null;` guard in `orders.tsx:18`, before the `getUserOrders` call. Re-ran the page, error gone, orders load once session resolves.

## Why it works
Reading the trace top to bottom and quoting real code at each frame stops the model from recognizing the error type ("Cannot read properties of undefined") and reaching for the first library-shaped explanation it's seen before. Tying the cause to one specific line and value keeps the fix targeted instead of defensive code sprayed everywhere.
