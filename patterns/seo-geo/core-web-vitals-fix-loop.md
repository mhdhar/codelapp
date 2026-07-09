---
title: "Core Web Vitals Diagnosis-to-Fix Loop"
slug: "core-web-vitals-fix-loop"
format: "loop"
category: "seo-geo"
tools: ["claude-code", "codex-cli"]
difficulty: "advanced"
est_time: "30 min"
models: ["any"]
summary: "Turn a failing Core Web Vitals report into a confirmed root cause and a shipped fix."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
PageSpeed Insights or CrUX data shows a Core Web Vitals metric (LCP, INP, CLS) failing on a real page, and you're working in the actual codebase. Use this instead of throwing speculative fixes — image compression, random lazy-loading — at a page without confirming what's actually causing the failure.

## The pattern
```text
A page served by this codebase is failing Core Web Vitals. The failing
page, the metric, and its current value are at the bottom, along with the
diagnostic data. Target the standard thresholds unless I say otherwise:
LCP under 2.5s, INP under 200ms, CLS under 0.1.

Work in this loop, in the actual codebase, do not guess:
1. Identify the single largest contributor to the failing metric from the
   diagnostic data (e.g. for LCP: which specific element is the LCP element
   and what's delaying it — render-blocking CSS, an unoptimized image, a
   late font swap, slow server response). Name the exact element and file.
2. Find that element/component in the codebase and read the actual code
   causing the delay. Show me the relevant lines.
3. Propose ONE fix targeted at that specific contributor. Do not bundle
   unrelated performance changes into the same fix.
4. Apply the fix.
5. State how to re-measure (rebuild, re-run Lighthouse/PageSpeed, or the
   specific command) and what value you'd expect if the fix worked.
6. If the metric is still failing after re-measurement, return to step 1
   and identify the next largest contributor. Do not repeat a fix that
   didn't move the number.

Stop when the metric is under target or you've been through 3 iterations,
whichever comes first. Report the before/after value at the end either way.

Failing page URL:
Failing metric and current value:
Paste the PageSpeed Insights / CrUX diagnostic data below this line:
```

## Real example output
```
Page: /product/checkout-flow, metric: LCP, current: 4.1s, target: <2.5s

Iteration 1
Step 1: LCP element is the hero image (largest-contentful-paint-element
in the trace points to img.hero-banner). Contributor: image is a 3.2MB
PNG served at native resolution, no responsive sizing, no priority hint.
Step 2: components/Hero.tsx:31 — plain <img src="/hero-full.png"> with no
next/image, no loading/priority attribute.
Step 3: Fix — replace with next/image, add priority, serve WebP.
Step 4: Applied. Hero.tsx:31 now uses <Image src="/hero-full.webp"
priority fill sizes="100vw" />.
Step 5: Rebuild and re-run `npx lighthouse https://staging.acme.com/product/checkout-flow --only-categories=performance`.
Expected LCP: ~2.6-2.9s (image no longer the bottleneck, font may be next).

Iteration 2
Step 1: Re-measured LCP 2.7s. New largest contributor: render-blocking
@import in globals.css loading Google Fonts before first paint.
Step 2: app/globals.css:2 — `@import url('https://fonts.googleapis.com/...')`.
Step 3: Fix — move to next/font with font-display: swap, drop the @import.
Step 4: Applied.
Step 5: Re-ran Lighthouse: LCP 2.2s. Under target.

Result: LCP 4.1s -> 2.2s in 2 iterations.
```

## Why it works
Core Web Vitals failures usually have one or two dominant causes, not ten. Forcing a single named contributor per iteration, with the actual code shown before a fix is proposed, stops the common failure mode of shipping five unrelated "performance improvements" that don't move the metric that's actually failing.
