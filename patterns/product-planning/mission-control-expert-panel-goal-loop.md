---
title: "Mission Control: Challenger, Builder, Orchestrator"
slug: "mission-control-expert-panel-goal-loop"
format: "goal"
category: "product-planning"
tools: ["universal"]
difficulty: "advanced"
est_time: "20 min"
models: ["any"]
summary: "Run an independent challenge-and-build panel, falsify its lead plan, then execute the smallest proven move."
author: "codel"
date: "2026-07-13"
license: "CC-BY-4.0"
aliases: ["master prompt", "mission mode", "expert panel", "project lead", "multi-agent debate", "challenger builder orchestrator"]
related: ["two-plan-bakeoff", "premortem-until-it-stops-dying", "smallest-useful-move-scoper"]
---

## When to use this

Use this for an important, ambiguous project where the first plausible plan is not trustworthy enough. It is especially useful when strategy, implementation, and approval boundaries must stay aligned through several evidence-producing steps.

## The pattern

```text
Treat the current request as one measurable mission. Take responsibility for moving it to a
verified outcome, while staying inside the authority already granted.

1. ORIENT
Inspect the available conversation, files, project instructions, repository state, tools,
prior decisions, and evidence. Record:
- requested outcome and user who benefits
- observable success signal
- constraints, authority, and unacceptable outcomes
- known facts, assumptions, inferences, unknowns, and the highest-risk uncertainty

Do not ask me to repeat available context. If one missing fact would materially change the
outcome or grant sensitive authority, ask one focused question and wait. Otherwise state the
safest reasonable assumption and continue.

2. REFRAME
Challenge whether the stated request solves the right problem. Write the original question,
the stronger question, and the measurable mission you will actually optimize. Answer both
questions when they materially differ.

3. RUN THE PANEL INDEPENDENTLY
Use these roles before seeking agreement:
- Challenger: attacks the premise, exposes opportunity cost and failure modes, and names the
  evidence that would disprove the plan.
- Builder: proposes the smallest coherent implementation, including dependencies, failure
  behavior, verification, rollout, rollback, and maintenance cost.
- Orchestrator: waits for the independent positions, preserves real dissent, compares options,
  and selects a path only after trying to falsify it.

If the environment can create separate agents or isolated contexts, run Challenger and Builder
independently, then give both results to the Orchestrator. If it cannot, run three clearly
separated lenses and label the result multi-lens analysis, not independent multi-agent evidence.
Each role must give a recommendation, the assumption it distrusts most, a challenge to another
role, and the evidence that would change its position.

4. RUN A FOCUS AUDIT
Classify every proposed capability as:
- CORE: directly creates the success signal
- ENABLING: required for core to work
- EXPERIMENTAL: useful only if a named hypothesis proves true
- DEFER: valuable later but not needed for this outcome
- REJECT: distracting, duplicative, unsafe, or outside the product's job

Cut or defer anything that cannot explain its connection to the mission. Sequence what remains
by dependency and time to evidence, not by excitement.

5. COMPARE AND FALSIFY
For strategic choices, compare at least three genuinely different options, including doing
nothing or using an existing system. Show user value, cost, complexity, risk, reversibility,
time to evidence, and delay cost. Choose a leader, present its strongest counterargument, and
try to prove it wrong. Replace it if another option survives better.

6. DEFINE THE EXECUTION CONTRACT
Before acting, state:
- smallest reversible evidence-producing step
- owner and dependencies
- done condition and verification method
- continue, pivot, stop, and escalation thresholds
- rollback boundary
- what remains explicitly out of scope

Never infer permission to push, merge, deploy, publish, delete, send external messages, spend
money, or mutate production data. Require explicit approval unless it is already present in the
current request.

7. EXECUTE AND LOOP
When implementation is requested and authorized, make the smallest coherent change, preserve
unrelated work, and verify it with targeted checks, broader relevant validation, and a user-facing
smoke test when possible. After each step, update the evidence and risk ledger. Repeat from the
focus audit only when new evidence changes the decision. Never loop merely to produce more text.

Required output:
1. Reframed mission and success signal
2. Panel positions and unresolved dissent
3. Focus audit and options
4. Decision, falsification attempt, and why the decision survived
5. Executed action and verification evidence
6. Biggest remaining risk, owner, and next threshold

End with: Best recommendation | Why it wins | Biggest risk | Immediate next action |
Evidence still needed.

Stop when the mission is verified, authority is missing, a material ambiguity needs my answer,
the risk is unsafe, existing work conflicts, or no new-evidence path remains. State the blocker
and safest next move instead of pretending progress.
```

## Real example output

```text
Mission: incorporate a 115-prompt Atlas into a copy-first agent-pattern catalog without slowing selection or duplicating content.

Challenger: Reject the bulk import. Existing patterns already cover assumption ledgers, dependency sequencing, pre-mortems, and adversarial review.
Builder: Ship a focused Mission Control pilot through the existing Markdown content database. Add no runtime or account system.
Orchestrator: Preserve the disagreement about a future prompt compiler, but defer it until the pilot proves demand.

Focus audit: CORE = one standalone Mission Control goal and outcome routing. ENABLING = existing search, related links, and catalog validation. EXPERIMENTAL = Quick/Mission compilation and usefulness ratings. DEFER = saved runs. REJECT = importing 115 entries or storing private project context.

Decision: publish the smallest pilot and measure selection and copy behavior. The plan survived the counterargument that a larger launch creates more SEO surface because that benefit does not outweigh duplication, untested claims, and slower choice.

Done: the new entry passes the content contract, appears in the public API and detail page, and its outcome link works in a browser. Pivot if users repeatedly combine several patterns or request a longer mission variant.
```

## Why it works

Independent challenge prevents the builder's implementation momentum from becoming the decision. The focus audit then turns debate into a small testable move, while explicit authority and stop rules keep persistence from becoming unsafe autonomy.
