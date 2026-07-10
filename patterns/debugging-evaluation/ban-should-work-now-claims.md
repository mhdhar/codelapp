---
title: "Ban 'Should Work Now' Claims"
slug: "ban-should-work-now-claims"
format: "prompt"
category: "debugging-evaluation"
tools: ["claude-code", "cursor", "codex-cli", "gemini-cli"]
difficulty: "beginner"
est_time: "2 min"
models: ["any"]
summary: "A standing rule that blocks an AI agent from claiming a fix works without pasting real output."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["verify-before-claiming-fixed", "self-verifying-build-loop", "no-fix-without-a-failing-test"]
---

## When to use this
Drop this in at the start of any debugging or coding session, once, so the agent can't drift into confident-sounding language like "this should fix it" or "try it now" for the rest of the session, instead of catching it after the fact every single time.

## The pattern
```text
Standing rule for this session: you may not say a bug is "fixed,"
"resolved," "should work now," or any equivalent phrase, unless the
message containing that claim also includes the real, pasted output of
you actually running the failing scenario and it now succeeds.

Specifically:
- Do not say "this should fix it" or "that should work now." If you
  haven't run it, say "I haven't run this yet" instead.
- Do not say "try it now" as a substitute for running it yourself when
  you have the ability to run it.
- If you can't run the original failing scenario (no test exists, no
  access to reproduce it), say that explicitly: "I can't verify this
  runs, here's what I'd need to check it." Do not imply confidence you
  don't have.
- If a run fails or produces unexpected output, report that plainly,
  even if it contradicts something you said earlier in this session.
- This rule applies for the rest of this session, not just the next
  message. Remind yourself of it before any message that claims
  something works.
```

## Real example output
Mid-session, after a config change to fix a build error:

"I haven't run this yet. Running `npm run build` now to check.

```
$ npm run build
> tsc && vite build
src/components/Sidebar.tsx:14:3 - error TS2322: Type 'string | undefined' is not assignable to type 'string'.
```

Still failing, different error than before, my change didn't fully fix it. Continuing."

Without the standing rule, the same session had earlier said "updated the tsconfig, that should resolve the build error" before running anything, and moved on to a different task.

## Why it works
The failure mode isn't dishonesty, it's that "should work" is the model's default confident register once a plausible-looking change is made, and by the time a human catches it the conversation has already moved three steps past the unverified claim. Stating the rule once, as a standing constraint rather than a one-time check, changes the language the agent reaches for at the moment it would normally claim success, instead of relying on the human to catch every instance after the fact.
