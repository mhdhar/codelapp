---
title: "Prioritize and Freshen a Sitemap for a Large Site"
slug: "sitemap-priority-for-large-sites"
format: "workflow"
category: "seo-geo"
tools: ["universal"]
difficulty: "advanced"
est_time: "20 min"
models: ["any"]
summary: "Turn a flat URL list into a segmented sitemap with accurate priority and lastmod."
author: "codel"
author_handle: ""
date: "2026-07-09"
license: "CC-BY-4.0"
---

## When to use this
Your site has thousands of URLs, sitemap.xml is one flat file with `priority=0.5` on everything, and crawl budget is being wasted on thin or dead pages. Use this before regenerating the sitemap, not after — the segmentation decisions drive the file structure.

## The pattern
```text
I'm restructuring my site's sitemap. Below the line is a sample export of
my pages (URL, page type, last content update date, 90-day organic
sessions). Take the domain from the URLs in the export. The approximate
total URL count is given at the bottom; if I leave it empty, treat the
sample as representative and say so in your output.

Do the following:
1. Group the URLs into logical sitemap segments by page type (e.g.
   /sitemap-products.xml, /sitemap-blog.xml, /sitemap-docs.xml). Tell me
   which segments should exist based on the page types actually present.
2. Flag URLs to EXCLUDE from the sitemap entirely: zero organic sessions
   AND no content update in 12+ months, obviously duplicate or paginated
   URLs (?page=2, ?sort=), and thin pages if that's inferable from the data.
3. Assign a <priority> value (0.1-1.0) per segment based on organic
   sessions density, not a flat guess. State the scale you used.
4. Set <changefreq> per segment based on how often that page type's content
   actually changes (blog vs. static legal pages vs. product pages).
5. Output a sitemap index file (sitemap.xml) referencing each segment file,
   plus one fully worked example segment file with 3-5 real URLs from the
   data, showing correct <lastmod>, <priority>, <changefreq>.

Use actual <lastmod> dates from the data provided. Do not use today's date
as a placeholder for pages that haven't changed.

Approximate total URL count:
Paste the page inventory export below this line:
```

## Real example output
```
brightpath.io — 4,200 URLs sampled. Segments identified:
- /sitemap-products.xml (1,800 URLs) — priority 0.8, changefreq weekly
  (highest session density: 62% of total organic sessions land here)
- /sitemap-docs.xml (1,300 URLs) — priority 0.6, changefreq monthly
  (steady traffic, content changes rarely once published)
- /sitemap-blog.xml (900 URLs) — priority 0.5, changefreq monthly
  (long tail, most posts get <10 sessions/quarter after month 2)
- /sitemap-legal.xml (12 URLs) — priority 0.2, changefreq yearly

Priority scale: 0.8 = top quartile by 90-day sessions, 0.5-0.6 = middle
half, 0.2-0.3 = bottom decile or evergreen/rarely-changing content.

Excluded (188 URLs): 142 paginated product listing pages (?page=N), 31
internal search result pages indexed by accident, 15 blog posts with 0
sessions and no update since 2023-11.
```
```xml
<!-- sitemap.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<sitemapindex xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <sitemap><loc>https://brightpath.io/sitemap-products.xml</loc></sitemap>
  <sitemap><loc>https://brightpath.io/sitemap-docs.xml</loc></sitemap>
  <sitemap><loc>https://brightpath.io/sitemap-blog.xml</loc></sitemap>
  <sitemap><loc>https://brightpath.io/sitemap-legal.xml</loc></sitemap>
</sitemapindex>

<!-- sitemap-products.xml (excerpt) -->
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://brightpath.io/products/invoice-automation</loc>
    <lastmod>2026-06-14</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
  </url>
</urlset>
```

## Why it works
A flat sitemap with uniform priority tells crawlers nothing — it's the equivalent of not setting priority at all. Segmenting by page type and deriving priority from actual traffic density gives crawlers a real signal about where to spend budget, and pruning dead URLs stops them from re-fetching pages that will never rank.
