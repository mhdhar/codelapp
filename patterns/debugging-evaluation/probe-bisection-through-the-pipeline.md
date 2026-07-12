---
title: "Probe Bisection Through the Pipeline"
slug: "probe-bisection-through-the-pipeline"
format: "workflow"
category: "debugging-evaluation"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "advanced"
est_time: "25 min"
models: ["any"]
summary: "Binary search a broken data flow with tagged log probes to find the exact step where the value goes wrong."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["log-diff-divergence-loop", "git-bisect-regression-hunt", "shrink-the-repro-until-it-confesses"]
---

## When to use this
A value comes out wrong at the end of a multi-step flow (handler to service to transform to store) and nothing throws, so there's no stack trace to read. Stepping through with a debugger isn't practical: the flow is async, spans processes, or only misbehaves under a real request.

## The pattern
```text
A value comes out wrong at the end of a data flow in this codebase.
Find the exact step where it goes wrong by bisecting the pipeline with
log probes.

Before adding a probe, confirm this is a local or otherwise approved
non-production environment. Never log credentials, tokens, personal data, or
full sensitive payloads; log a redacted or derived value that still tests the
hypothesis.

1. Trace the code path yourself, from the entry point to the place
   where the wrong value is observed. List it as an ordered chain of
   steps with file:line for each hop. Show me the chain before
   touching anything.
2. Pick a probe tag nobody else uses, like PROBE_BISECT. Every probe
   you add must log that tag plus the step name plus the redacted or
   derived value under suspicion.
3. Insert one probe at the midpoint of the chain. Run the repro once.
   If the value is still correct at the midpoint, the corruption is
   downstream: bisect the second half. If it's already wrong, bisect
   the first half.
4. Repeat step 3 until two adjacent probes straddle the bug: value
   correct going into one step, wrong coming out of it. Leave earlier
   probes in place between runs, the full probe trail is your evidence.
5. Read that one step's code and state the cause, tied to the specific
   line and the probe values on either side of it. If the step calls
   out to code you can't probe (a library, a network hop), say so and
   name what evidence you'd need next.
6. Paste the final redacted probe trail from the last run as proof, then
   delete every probe and verify cleanup by searching the repo for the tag.
   Do this cleanup even if the repro fails or the investigation stops early;
   the search must come back empty before you're done.

Do not fix anything in this pass. Locate, prove, clean up, report.

First, ask me in one message what comes out wrong: expected vs. actual,
and where I can see it. Wait for my reply before touching anything.
```

## Real example output
Symptom: order total shows $1,204.00 in the confirmation email, checkout page correctly shows $12.04.

Step 1: chain has 7 hops, from `POST /checkout` (`routes/checkout.ts:31`) through `buildOrder`, `applyDiscount`, `toEmailPayload`, to `renderEmail` (`emails/order.tsx:88`).

Runs 1 to 3, probe trail from the final run:
```
PROBE_BISECT applyDiscount.out total=1204 (cents)
PROBE_BISECT toEmailPayload.in total=1204 (cents)
PROBE_BISECT toEmailPayload.out total=1204 (expected dollars)
```

Adjacent probes straddle `toEmailPayload` (`src/email/payload.ts:44`): it receives cents and passes the number through, but `renderEmail` formats its input as dollars. Every other consumer of `applyDiscount` divides by 100 at the boundary; this one skips it. Cause: missing cents-to-dollars conversion at `payload.ts:44`, introduced when the email path was split off from the receipt path.

Cleanup: removed 3 probes, repo search for `PROBE_BISECT` returns 0 matches.

## Why it works
Bisection replaces "read all seven steps carefully" with three cheap runs, and the probe trail turns the diagnosis into evidence anyone can check: correct value in, wrong value out, one step between them. The unique tag makes the instrumentation disposable by construction, a single search proves the debugging scaffolding all came back out.
