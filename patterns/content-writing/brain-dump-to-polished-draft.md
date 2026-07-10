---
title: "Brain Dump to Polished Draft"
slug: "brain-dump-to-polished-draft"
format: "prompt"
category: "content-writing"
tools: ["universal"]
difficulty: "beginner"
est_time: "10 min"
models: ["any"]
summary: "Turn messy notes into a publish-ready blog post or landing page section in one pass."
author: "codel"
author_handle: ""
date: "2026-07-08"
license: "CC-BY-4.0"
related: ["opening-hook-rewrite", "strip-ai-voice", "skeptical-reader-gauntlet"]
---

## When to use this
You have a wall of half-formed thoughts, bullet points, or a voice memo transcript about a topic and need a real draft. You know what you want to say but not how to structure or phrase it.

## The pattern
```text
Turn my rough notes into a polished draft.

Rules:
- Keep every factual claim from my notes. Do not invent statistics, quotes, or examples.
- Organize into a clear structure with a strong opening line that states the point immediately.
- Cut anything that doesn't serve the reader taking action or understanding the point.
- Match the voice exactly. If my notes are casual, stay casual. If technical, stay technical.
- Flag anywhere you had to guess or a gap needs my input, inline, as
  (NEEDS INPUT: what's missing).

Output the full draft only. No preamble, no explanation of what you did.

First ask me in one message for: (1) my raw notes, (2) the content type
(e.g. blog post, landing page section), (3) the voice (e.g. "direct,
confident, no fluff, second person"). Then wait. If I skip (2) or (3),
infer the most natural choice from the notes themselves and say what
you chose in one line before the draft.
```

## Real example output
"Most teams don't have a deploy problem. They have a confidence problem. You can ship code in five minutes and still take an hour to actually press the button, because nobody trusts the pipeline to tell them the truth. (NEEDS INPUT: confirm this framing matches the intended audience level) The fix isn't more process. It's a pipeline that fails loud and fails fast, so the default answer to 'can I ship' becomes yes."

## Why it works
Separating voice instructions from content prevents the model from defaulting to generic AI phrasing. The "flag gaps" rule stops it from quietly inventing facts to fill holes in your notes, which is the main failure mode of one-shot drafting prompts.
