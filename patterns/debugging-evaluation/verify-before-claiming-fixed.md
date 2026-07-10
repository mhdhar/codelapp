---
title: "Verify Before Claiming Fixed"
slug: "verify-before-claiming-fixed"
format: "loop"
category: "debugging-evaluation"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "beginner"
est_time: "5 min"
models: ["any"]
summary: "Don't accept 'fixed it' from an AI agent until it re-runs the exact failing case and shows real output."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
related: ["ban-should-work-now-claims", "adversarial-fix-verification-loop", "no-fix-without-a-failing-test"]
---

## When to use this
An AI agent just said "fixed" or "done" after changing code. You want proof, not a claim, before you move on or merge.

## The pattern
```text
You just said this is fixed/done. Before I accept that, do the following:

1. Re-run the exact command or test that originally failed. Use the same
   input, same flags, same environment. Do not run a different or
   simplified version.
2. Paste the full, real output of that run, not a summary or paraphrase.
3. If the output shows success, quote the specific line(s) that prove it
   (e.g. "PASS", exit code 0, the expected value in the response).
4. If the output shows any failure, warning, or unexpected result, say so
   plainly and do not call it fixed. Go back and continue debugging.
5. If you cannot re-run the original scenario (no test exists, no way to
   reproduce), say that explicitly instead of claiming success.

Only after showing real passing output should you say the issue is resolved.
```

## Real example output
Re-ran `pytest tests/test_auth.py::test_expired_token -v`.
Output:
```
tests/test_auth.py::test_expired_token PASSED [100%]
1 passed in 0.42s
```
The PASSED line and 0 failures confirm the expired token now returns a 401 instead of a 500. Marking this fixed.

## Why it works
AI agents optimize for sounding done, not being done. Requiring the exact original failing scenario, with pasted real output, closes the gap between "I believe this works" and "I watched it work." It also catches cases where the agent quietly changed the test instead of the code.
