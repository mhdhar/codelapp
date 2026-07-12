---
title: "Audit Page Copy for AI Citation-Worthiness"
slug: "ai-citation-worthiness-audit"
format: "goal"
category: "seo-geo"
tools: ["universal"]
difficulty: "intermediate"
est_time: "20 min"
models: ["any"]
summary: "Assess which sentences are clear, query-relevant, and supportable enough to be useful to answer engines."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["generate-llms-txt", "concrete-example-injector", "entity-consistency-audit"]
---

## When to use this
Your page ranks and loads fine, but you want to make its claims more useful to answer engines. Use this to separate clear, query-relevant, supportable claims from generic marketing copy. This is an editorial readiness check, not a prediction that a particular AI product will cite the page.

## The pattern
```text
Audit the body copy of one page from my site against a target query. This
is a transparent editorial heuristic, not a forecast of whether any AI
engine will quote or cite the page:
1. Go paragraph by paragraph. Mark each one READY, GENERIC, or NEEDS EVIDENCE.
   READY means it gives a direct, query-relevant answer with a specific claim
   supported by the page or source material I supplied. GENERIC means it is a
   vague benefit statement, an adjective stack ("seamless, powerful,
   industry-leading"), or lacks useful detail. NEEDS EVIDENCE means it has a
   potentially useful claim but its precision or support cannot be verified.
2. For every GENERIC or NEEDS EVIDENCE paragraph, say exactly what evidence,
   fact, or detail would be needed to strengthen it. Do not infer plausible
   facts or invent statistics.
3. Rewrite the 3 weakest GENERIC paragraphs using only supportable facts
   already present on the page or in the supplied sources. Where proof or a
   number is missing, write "NEEDS DATA:" followed by what to supply.
4. Check whether the page has one clear, one-sentence definition of the
   core topic near the top that directly answers the target query. If not,
   write a candidate based only on the supplied page and sources.
5. Give an editorial readiness score out of 10 for the page as a whole,
   based on query fit, answer clarity, specificity, and evidence. State the
   single biggest reason it is losing points; do not present the score as a
   citation prediction.

Start by asking me in one message for (1) the target query, (2) the page's
body copy pasted in, and (3) the source material that supports its factual
claims, if available. Wait for my reply before doing anything.
```

## Real example output
```
Paragraph 1: "Acme's invoicing platform helps growing businesses manage
their finances more efficiently and stay on top of cash flow."
-> GENERIC. Missing: what "more efficiently" means in a number (time
saved, error rate, days-to-payment). No specific mechanism named.

Paragraph 2: "Automated matching reconciles line items against purchase
orders in under 2 seconds per invoice, catching mismatches before they
reach approval."
-> NEEDS EVIDENCE. The mechanism and timing are useful, but the timing needs
supporting data before it can be treated as a reliable claim.

Paragraph 3: "Teams using Acme report significant time savings on
month-end close."
-> GENERIC. Missing: how many teams, what "significant" means in hours
or days, and whether this is a survey stat or a case study.

Rewrite of Paragraph 1:
"Acme automatically reconciles invoice line items against purchase orders
before approval. NEEDS DATA: verified review-time or mismatch-reduction result,
with its source."

Rewrite of Paragraph 3:
"NEEDS DATA: teams using Acme reduce month-end close by (fill in: days or
hours saved), based on (fill in: survey size or internal usage data)."

Candidate top-of-page definition (verify against product scope): "Invoice
automation uses software to match, validate, and route invoices for approval
without manual line-item review."

Editorial readiness score: 4/10. Biggest reason: the page has few direct,
specific claims and the supplied copy includes no evidence for its performance
statements. This score is a copy-review heuristic, not a citation forecast.
```

## Why it works
Clear, query-relevant claims with available evidence are more useful than generic benefit language. Separating READY, GENERIC, and NEEDS EVIDENCE makes the missing proof visible before a polished sentence turns into an unsupported claim.
