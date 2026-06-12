---
name: H&M marketing website (hmjunkland-site-v2) — build state & improvement backlog
description: The Desktop static site for H&M Junk Removal; structure, conventions, and ranked ideas/issues to keep improving
type: project
---

Static marketing site at `C:\Users\Miles Dickey\Desktop\hmjunkland-site-v2\` (SEPARATE from mmm-tool-v1 working dir).
Repo: `github.com/milesdcatt-dev/hm-landscaping-and-junk-removal-` (branch main, plain git, NO gh CLI).
Plain HTML + CSS + vanilla JS (script.js) + GSAP/ScrollTrigger CDN. No build step. Preview via the Claude
Preview MCP (serverId changes per session; reload with `window.location.href='/index.html?cb='+Date.now()`).

**Why:** Miles's real business site; he iterates on it heavily with Claude, often overnight in "full edit mode"
("keep editing and make it better and better till we have no usage"). Honesty is the #1 rule (see below).

**How to apply:** Edit ONLY this Desktop dir. Verify every change in the preview (DOM `preview_eval` is
authoritative — the screenshot tool often collapses to a narrow strip). Commit AND push each verified change.

## Hard rules (from reference_hm_brand + Miles)
- HONEST brand: show the REAL crew/truck. NO fake reviews, stats, customer names, or fabricated specifics.
  The reviews section is an honest "building reviews one neighbor at a time" invite, NOT fake testimonials.
- No em/en dashes in customer-facing copy (use commas or ·). Avoid: Premier, Elite, Industry-leading,
  World-class, and "transform"/"transformation" in copy.
- Phone 202-999-7885, site hmjunkland.org. Primary CTA "Text a pic for a free quote."

## Page structure (top → bottom)
Hero (cover panel + framed crew photo + logo pill — Phase 1 work, considered DONE/locked), trust-bar, marquee,
#services (6 svc-cards), #flow (chat mockup + 4 steps), #why (6 cards), #areas (pill grid + skyline SVG),
#about (copy + crew photo), #gallery (Before&After: truck-feature + "what we haul" aside + 3 real B/A cards +
CTA panel), #reviews (honest invite), #faq (6 native <details>), final-cta, footer, sticky mobile-bar.
Kicker numbers: services=01 … reviews=07, faq=08, final-cta=09.

## Real gallery assets (treated as real H&M work per Miles)
truck-loaded.jpg (loaded Tacoma), ba-powerwash.webp (paver wash), ba-mulch.jpg (mulch bed),
ba-cleanout.webp (shed/garage cleanout, navy frame auto-cropped). Captions kept truthful/generic — NO
fabricated specifics. Optimize new images with PIL (system Python 3.12 has Pillow): JPEG q82-84 progressive,
WebP q82-84 method 6, LANCZOS resize; always add width/height + loading="lazy".

## DONE (2026-06-10)
- Hero polish (Phase 1): crew uncropped, panel/photo gap, +15% photo, outlined+bold heading w/o periods, logo pill.
- Gallery rebuilt as honest Before&After; images optimized (truck 2.3MB→226KB, mulch 1.5MB→242KB, cleanout 104KB).
- FAQPage JSON-LD added (mirrors the 6 visible FAQs). LocalBusiness JSON-LD already existed.
- Fixed og:image/twitter:image to ABSOLUTE URLs (relative breaks FB/Twitter scrapers); added og:image:alt;
  width/height on remaining gallery imgs to cut CLS.
- Backups in `_backups/` (timestamped index/styles snapshots).

## Backlog — needs an asset from Miles (no code substitutes)
1. More REAL before/after job photo pairs (the #1 unlock for the gallery — drop them in assets/).
2. Real customer reviews (text-in) to replace the honest placeholder with real quotes.
3. Confirm real "from $X" starting prices if he wants pricing shown anywhere.

## Backlog — pure code, free, can do unattended (ranked)
4. Add width/height to the 6 service-card imgs + the about/crew photo for full CLS coverage (low risk, they
   sit in aspect-ratio containers so impact is small).
5. Consider a tiny sitemap.xml + robots.txt for SEO (only if hosting serves them at root).
6. Reduced-motion: ensure GSAP reveals respect prefers-reduced-motion (verify script.js gates animations).
7. Lazy-load offscreen CDN (GSAP already defer'd) — fine; revisit only if Lighthouse flags.
8. Accessibility pass: focus-visible styles on all buttons/links, alt text audit, color-contrast check on
   sage/cream text.
9. Self-host fonts (currently Google Fonts CDN) only if Miles wants zero third-party calls — minor.

## Gotchas
- `loading="lazy"` imgs below the fold report loaded:false / 0x0 in DOM until scrolled — scroll the section
  into view before asserting they loaded.
- Screenshot MCP tool unreliable here (narrow strip) — trust preview_eval getBoundingClientRect/naturalWidth.
- git warns "LF will be replaced by CRLF" on index.html — harmless on Windows.
