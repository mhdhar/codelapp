---
title: "Repurpose One Post Into a Multi-Format Pack"
slug: "repurpose-post-into-multi-format-pack"
format: "workflow"
category: "content-writing"
tools: ["universal"]
difficulty: "intermediate"
est_time: "20 min"
models: ["any"]
summary: "Turn one long-form post into a thread, a LinkedIn post, and an email blurb, not chopped-up excerpts."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
You wrote one solid long-form piece and need it to also work as a thread, a LinkedIn post, and a short email teaser. A straight copy-paste-and-cut job never sounds native to any of them.

## The pattern
```text
Step 1: Extract the core argument.
"Read the post I paste below and state its single core argument in one
sentence, plus the 3 supporting points that prove it. Nothing else.
Paste the post below this line:"

Step 2: Review the extraction. Correct it if the model picked a
secondary point over the actual main argument.

Step 3: Generate each format from the extraction, not the original post.
"Using the core argument and 3 supporting points from step 1, write
three separate pieces:
1. An X thread of 6-8 posts, each under 280 characters, one point per
   post, hook in post 1, no numbering like '1/8'. If I name a different
   platform or character limit, use that instead.
2. A LinkedIn post under 200 words: more context and personal framing
   than the thread, ends with a question that invites comments.
3. A 3-sentence email teaser that makes someone click through to the
   full post, doesn't give away the ending."

Step 4: Read each output against the original post to confirm no claim
got distorted or overstated in compression.
```

## Real example output
Core argument (from Step 1): "Code review comments that ask questions produce better code than comments that issue commands, because commands get complied with instead of understood." Supporting points: commands trigger compliance not comprehension; questions force the author to defend or discover the flaw themselves; teams that switch to question-based review have fewer repeat mistakes.

Thread, post 1: "Your last review comment said 'use a Map here instead of nested loops.' The author changed it. They also didn't learn anything, and made the same mistake in the next PR."

LinkedIn post (opening): "I used to think good code review meant catching every issue before it merged. Then I noticed the same three engineers kept making the same three mistakes, no matter how many times I caught them..."

Email teaser: "Most 'improve your code review' advice is about catching more bugs. The teams with the fewest repeat mistakes are doing the opposite: catching fewer, but making sure the author actually understands why."

## Why it works
Extracting the argument before drafting each format stops the model from anchoring on the original post's sentence structure and just trimming it, which is how you end up with a "thread" that's really six sequential excerpts. Rebuilding from the same core argument means all three pieces stay accurate to each other even though they read completely differently.
