---
title: "Fix Broken Schema Markup From Validator Errors"
slug: "schema-markup-validation-fix"
format: "prompt"
category: "seo-geo"
tools: ["universal"]
difficulty: "intermediate"
est_time: "10 min"
models: ["any"]
summary: "Give it your JSON-LD plus the validator's error output, get back a corrected block."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
related: ["jsonld-from-page-content", "faq-schema-from-support-content", "error-message-forensics-loop"]
---

## When to use this
Google's Rich Results Test, the Schema.org validator, or Search Console's Enhancements report is flagging errors or warnings on schema markup that's already live. Use this to fix it. For generating new schema from scratch, see jsonld-from-page-content instead.

## The pattern
```text
One of my pages has live JSON-LD structured data that a validator is
flagging. I'll give you the JSON-LD, then a line reading "VALIDATOR
ERRORS:", then the exact error/warning output from the validator.

Fix the schema:
1. Go through each validator error/warning one at a time. State what it
   means in plain language and which property it points to.
2. Fix each one using only information that is plausibly true of this page
   (infer from the existing JSON-LD's other fields and the page context
   given). If a real value is missing, never put a `NEEDS:` placeholder in
   JSON-LD: omit an optional property, or omit the affected object when a
   valid value is necessary. List the missing fact outside the code block.
   Do not fabricate ratings, prices, or review counts to silence a validator
   warning.
3. Do not change any property the validator did not flag, even if you'd
   phrase it differently.
4. Output a single deployable JSON-LD script block containing only valid,
   supported values; if no deployable block is possible, say so instead of
   emitting placeholder JSON-LD.
5. After the block, list which errors are now fully fixed versus which
   still need a real value from me before they'll pass.

Start by asking me in one message for the page URL, the JSON-LD, and the
validator output after a "VALIDATOR ERRORS:" line. Wait for my reply
before fixing anything.
```

## Real example output
```
Validator errors on /products/standing-desk-pro:
1. "Missing field 'reviewCount' (in 'aggregateRating')" - AggregateRating
   is present but has 'ratingValue' with no 'reviewCount', which Google
   requires together.
2. "Invalid value in field 'priceValidUntil' (expected date, got empty
   string)" - priceValidUntil is present as "" instead of a real date or
   omitted entirely.
3. "The 'image' field should be a URL, not an array of objects" - image
   is currently [{ "url": "..." }] instead of a plain string or array of
   URL strings.
```
```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "Standing Desk Pro",
  "image": [
    "https://acme.com/img/standing-desk-pro-1.jpg",
    "https://acme.com/img/standing-desk-pro-2.jpg"
  ],
  "offers": {
    "@type": "Offer",
    "price": "499.00",
    "priceCurrency": "USD"
  }
}
</script>
```
```
Fixed: error 3 (image field now a plain array of URL strings, matching
what was already on the page).
Still needs your input: error 1 (reviewCount: pull the real count from
your reviews backend before restoring aggregateRating). Error 2 is fixed
by omitting priceValidUntil because no actual price-expiration date was supplied.
```

## Why it works
Validator errors are usually a shape problem (wrong type, missing sibling property) or a missing-data problem, and they need different responses: fixing the shape is safe to automate, inventing the missing number is not. Flagging what still needs a real value keeps the output from passing validation with fabricated data that Google can later penalize as misleading.
