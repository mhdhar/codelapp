---
title: "Proposal Draft From a Short Brief"
slug: "proposal-draft-from-brief"
format: "workflow"
category: "content-writing"
tools: ["universal"]
difficulty: "intermediate"
est_time: "20 min"
models: ["any"]
summary: "Turn a short project brief into a structured client proposal or RFP response draft."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
---

## When to use this
A prospect sent you a brief or RFP and you need a first-draft proposal fast, with the right structure, before you spend real time customizing the specifics.

## The pattern
```text
Step 1: Paste the brief and generate the skeleton.
"Read this brief and outline a proposal with these sections: Problem
Understanding, Approach, Scope, Timeline, Investment. For each section,
write 2-3 bullet points of what should go there based on the brief. Do
not write full prose yet. Brief: [PASTE BRIEF]"

Step 2: Review the outline. Correct any misread requirements or add
missing constraints (budget ceiling, hard deadline, must-have tech).

Step 3: Expand to full draft.
"Expand this outline into a full proposal draft. Problem Understanding
should mirror the client's own language back to them, proving we
listened. Approach should explain the 'how' in plain language, no
jargon. Scope should be a bulleted list of concrete deliverables, not
vague promises. Timeline should use relative phases (Phase 1, Phase 2)
not fixed dates unless I gave you a start date. Leave [RATE] and
[START DATE] as placeholders for me to fill in. Outline: [PASTE
OUTLINE FROM STEP 1]"

Step 4: Read the draft against the original brief line by line.
Confirm every stated requirement is addressed somewhere in Scope.
```

## Real example output
"Problem Understanding: You're maintaining two separate checkout flows for web and mobile, and every pricing change means updating both, which is where last quarter's discount bug came from. Scope: Unify checkout logic into a single shared service. Migrate web checkout to consume it. Migrate mobile checkout to consume it. Add a test suite that catches pricing mismatches before deploy."

## Why it works
Generating the skeleton first lets you catch misread requirements before the model burns effort writing full prose around a wrong assumption. Mirroring the client's language in Problem Understanding is what makes a proposal read as heard rather than templated.
