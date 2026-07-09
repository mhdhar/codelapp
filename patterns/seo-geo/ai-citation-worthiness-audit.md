---
title: "Audit Page Copy for AI Citation-Worthiness"
slug: "ai-citation-worthiness-audit"
format: "goal"
category: "seo-geo"
tools: ["universal"]
difficulty: "intermediate"
est_time: "20 min"
models: ["any"]
summary: "Find which sentences on a page an AI engine would actually quote, then fix the rest."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
Your page ranks and loads fine but never gets quoted in ChatGPT, Perplexity, or an AI Overview answer. Use this to separate specific, citable claims from generic marketing fluff that answer engines skip straight over.

## The pattern
```text
Here is the body copy of a page targeting the query "[TARGET_QUERY]":

[PAGE_COPY]

Audit it for AI citation-worthiness:
1. Go paragraph by paragraph. Mark each one CITABLE or GENERIC. CITABLE
   means it states a specific number, a named fact, a precise definition,
   or a concrete claim an answer engine could quote verbatim and be right.
   GENERIC means it's a vague benefit statement, an adjective stack
   ("seamless, powerful, industry-leading"), or a claim with no specific
   detail attached.
2. For every GENERIC paragraph, say exactly what specific fact, number, or
   detail is missing that would make it citable, based only on what's
   plausible given the rest of the page. Don't invent stats I don't have.
3. Rewrite the 3 weakest GENERIC paragraphs into citable versions, using
   real facts already present elsewhere on the page where possible, and
   marking [NEEDS DATA] anywhere a real number I haven't given you would
   be required.
4. Check whether the page has one clear, quotable one-sentence definition
   of the core topic near the top — the kind of sentence an AI Overview
   would lift directly. If not, write one.
5. Give a citation-worthiness score out of 10 for the page as a whole, and
   the single biggest reason it's losing points.
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
-> CITABLE. Specific mechanism (line-item matching) and a real number
(under 2 seconds).

Paragraph 3: "Teams using Acme report significant time savings on
month-end close."
-> GENERIC. Missing: how many teams, what "significant" means in hours
or days, and whether this is a survey stat or a case study.

Rewrite of Paragraph 1:
"Acme's line-item matching engine cuts invoice review time from an
average of 12 minutes to under 90 seconds per invoice by auto-reconciling
against purchase orders."

Rewrite of Paragraph 3:
"[NEEDS DATA]: teams using Acme reduce month-end close by [X days/hours],
based on [survey of N customers / internal usage data]."

Top-of-page definition (missing, added): "Invoice automation is the use
of software to match, validate, and route invoices for approval without
manual line-item review."

Citation-worthiness score: 4/10. Biggest reason: only 1 of 5 paragraphs
contains a checkable number; the rest read as benefit statements an
answer engine has no reason to quote over a competitor's page.
```

## Why it works
Answer engines quote sentences that reduce their own risk of being wrong — specific numbers and precise definitions are easy to attribute and hard to dispute. Splitting a page into CITABLE and GENERIC forces you to see exactly how much of the copy is doing marketing work versus giving an AI engine something safe to repeat.
