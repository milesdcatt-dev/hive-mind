# Miles Project Memory

## Main Projects

### 1. H&M Junk Removal & Landscaping Website
- Main site folder:
  `C:\Users\Miles Dickey\Desktop\hmjunkland-site-v2`
- Local page often opened directly:
  `file:///C:/Users/Miles%20Dickey/Desktop/hmjunkland-site-v2/index.html`
- Local server URL used before:
  `http://127.0.0.1:8765/`
- Site type:
  Static single-page website.
- Main files:
  - `index.html`
  - `styles.css`
  - `script.js`
  - `loader.css`
  - `loader.js`
- Brand:
  H&M Junk Removal & Landscaping.
- Domain:
  `hmjunkland.org`
- Phone:
  `202-999-7885`
- Business:
  Local Northern Virginia junk removal, hauling, yard cleanup, mulch, power washing, cleanouts.
- Voice:
  Premium but local, warm, honest, student-run, hardworking, trustworthy.
- Avoid:
  Corporate SaaS feel, fake luxury language, clutter, generic AI-looking layouts, fake stats, fake reviews, awkward copy.

## H&M Brand Details
- Palette:
  - Forest green
  - Warm cream
  - Construction orange
  - Charcoal
  - Sage
- Existing CSS variables include:
  - `--green-700: #1F4D33`
  - `--orange: #F47A20`
  - `--bg: #F5F1E6`
  - `--ink: #1F1F1F`
- Fonts:
  - Manrope for display
  - Inter for body
  - Bebas Neue for numbers/badges
- Assets folder:
  `C:\Users\Miles Dickey\Desktop\hmjunkland-site-v2\assets`
- Important assets:
  - `logo-primary.png`
  - `logo-badge.png`
  - `favicon.png`
  - `hm-truck.jpg`
  - `crew.jpg`
  - `svc-cleanout.jpg`
  - `svc-yard.jpg`
  - `svc-mulch.jpg`
  - `svc-power.jpg`
  - `mower-guy.png`

## H&M Site Sections
- Loader:
  Custom branded mower/grass/loader animation using `loader.css` and `loader.js`.
- Header:
  Logo, nav, phone pill, text quote CTA, mobile drawer.
- Hero:
  Crew/truck visual, "Hard work. Clean results." style.
- Trust bar:
  Local/student-run, service area, photo quotes, upfront pricing.
- Marquee:
  Services like junk removal, cleanouts, hauling, power washing.
- Services:
  Card grid for junk removal, landscaping/mulch, yard cleanup, power washing, cleanouts, small hauling.
- How It Works:
  Recently rebuilt with supplied PNG pack.
- Why H&M:
  Trust/benefit cards.
- Areas:
  Northern Virginia areas.
- About:
  Crew story and local owner/operator angle.
- Reviews:
  Honest review invitation, no fake review claims.
- FAQ:
  Quote, service area, timing, pricing, items not accepted, minimum job size.
- Final CTA:
  Text photos / call.

## Current H&M "How It Works" Work

### Asset Pack
- ZIP provided:
  `C:\Users\Miles Dickey\Downloads\hm_how_it_works_png_asset_pack.zip`
- Extracted to:
  `C:\Users\Miles Dickey\Desktop\hmjunkland-site-v2\public\assets\hm-how-it-works`
- Important files:
  - `00_full_section_reference.png`
  - `01_bg_dark_green_gradient.png`
  - `02_headline_glass_backplate.png`
  - `03_step_card_backplate.png`
  - `04_trust_pill_base.png`
  - `05_cta_orange_button_base.png`
  - `07_icon_photo_upload.png`
  - `08_icon_quote_doc.png`
  - `09_icon_calendar.png`
  - `10_icon_truck.png`
  - `11_phone_float_2_5d.png`
  - `12_phone_shadow_glow.png`
  - `13_left_leaf_grass_cluster.png`
  - `14_upper_right_leaf_glow.png`
  - `15_right_paver_path_texture.png`
  - `16_warm_sunlight_corner_glow.png`
  - `17_left_foliage_soft_overlay.png`
  - `18_phone_remade_front.png` was generated from the original phone PNG with padding.

### How It Works Requirements
- All readable text should be real HTML text, not baked into PNGs.
- PNGs are only visual layers/assets.
- Section should look like the reference but be responsive and clean.
- Desktop:
  - Dark green full background.
  - Top headline centered inside glass backplate.
  - Phone on left.
  - Process cards on right.
  - Trust chips below cards.
  - CTA under chips.
- Mobile:
  - Stack headline, phone, steps, CTA.
  - Phone centered.
  - Cards full width.
  - Foliage reduced so it does not cover text.

### Exact How It Works Text
- Label:
  `02 HOW IT WORKS`
- Headline:
  `Text Photos. Get a Clear Quote. Cleared.`
- `Cleared.` should be orange and italic.
- Current shortened subheadline:
  `Send a few photos. Get one clear by-the-job price before we lift a thing.`
- Original longer subheadline:
  `No in-person estimate. No hourly surprises. Send a few photos and see the by-the-job price before we lift a thing.`

### Steps
1. Heading:
   `Send a few photos`
   Body:
   `Text the pile, the yard, the driveway, or whatever needs to go. More angles mean a tighter quote.`
2. Heading:
   `Get a clear quote back`
   Body:
   `One number, by the job. Written, not a phone guess. Disposal and clean-up baked in.`
3. Heading:
   `Pick a time`
   Body:
   `Same-day and next-day spots are often available. We work around your schedule.`
4. Heading:
   `We haul, clean, leave it better`
   Body:
   `Loaded, hauled, swept up. You should only notice what's gone.`

### Trust Chips
- `No hourly surprises`
- `Fast photo quotes`
- `Clear price up front`

### CTA
- `Text Photos for a Free Quote →`
- Helper:
  `Most quotes only need 3-5 photos.`

## Recent H&M Fixes / Pain Points
- User disliked the phone PNG placement because:
  - phone clipped at bottom
  - fake CSS phone looked bad
  - fake rear/side ghost phone appeared in background
  - phone was facing wrong direction
  - phone was not sitting on the shadow/glow correctly
- Current state:
  - CSS-built phone was removed.
  - Phone now uses remade PNG asset:
    `18_phone_remade_front.png`
  - HTML now uses:
    - `12_phone_shadow_glow.png`
    - `18_phone_remade_front.png`
- User still wants careful visual placement, not janky approximations.
- User wants Codex to spend more time on visual fidelity instead of quick patches.

## Latest H&M Header Fix
- User wanted headline text to sit inside the glass backplate.
- User wanted pill color changed so it does not match the background.
- User wanted useless text cut down.
- Changes made:
  - Glass backplate made taller.
  - Heading block constrained.
  - H1 reduced slightly.
  - Label pill changed to cream/orange.
  - Subheadline shortened.

## Built By M / BuiltByMMM Website
- Separate project discovered in old worktree:
  `C:\Users\Miles Dickey\Desktop\mmm-tool-v1\.claude\worktrees\elastic-bhabha\builtbym.html`
- Brand:
  Built By M / BuiltByMMM.
- Purpose:
  Website design / web services business for local businesses.
- Contact:
  - Phone: `703 447 2311`
  - Email: `builtbymmm@gmail.com`
- Desired style:
  Modern, warm, trustworthy, local, premium, less sloppy.
- Important instruction:
  No em dashes in copy.
- BuiltByM site issues from previous prompt:
  - Reformat top bar.
  - Remove bad Command-K symbol.
  - Update phone number to `703 447 2311`.
  - Remove green status dot.
  - Blend logo PNG into background so no box is visible.
  - Add logo/name near hero.
  - Make "modern websites for local businesses" smaller under name.
  - Fix double periods.
  - Make hero highlight feel like soft rounded/bubble text.
  - Humanize copy.
  - Fix blocky pricing/stat row.
  - Reduce loud blue strip.
  - Remove duplicate/redundant strips.
  - Rework "Type your name. See 5 sites instantly."
  - Pricing should be:
    - Basic: `$999 one-time`
    - Standard: `$1499 one-time`
    - Premium: `$1999 one-time`
  - Move "three promises" into pricing/payment area.
  - Delete standalone "Three things we promise" section.
  - Remove duplicate "What we see on most sites we replace" section.
  - Connect all email/form behavior to `builtbymmm@gmail.com`.
- Suggested BuiltByM copy:
  - Hero/subtitle:
    `Modern websites for local businesses, built fast and handled for you.`
  - Value:
    `We build, host, and manage your website for a simple monthly price, so you get a real person helping you without agency-level costs.`
  - Stats:
    `From $299 setup. Live in 7 days. Real human support. No hidden monthly surprises.`
  - Preview:
    `Type your business name and preview site ideas instantly. It is not your final website, but it shows how fast we can get started.`
  - Promises:
    `You approve before you pay. 30 days, full refund. A real human answers.`
  - FAQ intro:
    `Straight answers to the questions almost every business owner asks before starting.`
  - Contact CTA:
    `Tell us what you need. We will send back a real preview and a simple next step.`

## MMM Tool / Lead Dashboard Project
- Main workspace:
  `C:\Users\Miles Dickey\Desktop\mmm-tool-v1`
- App type:
  Python Flask local lead generation/dashboard tool.
- Main files:
  - `dashboard.py`
  - `scrape_leads.py`
  - `scrape_routes.py`
  - `ad_routes.py`
  - `ad_generator.py`
  - `db.py`
  - `boot.py`
  - `call_script.html`
  - `README.md`
- Database:
  `leads.db`
- Purpose:
  Find local businesses with no, broken, or low-quality websites and manage outreach.
- Dashboard:
  Local Flask app usually at:
  `http://localhost:5000`
- Scraper:
  Uses Google Places, website analysis, social lookup, scoring, and SQLite/CSV migration history.
- Lead criteria:
  - no website
  - broken SSL
  - connection error
  - timeout
  - 404/gone/server error
  - Facebook only
  - live but poor quality
- Website quality flags:
  - no SSL
  - slow
  - no mobile
  - weak SEO
  - no contact
- Lead dashboard features:
  - auth/login
  - SQLite-backed leads
  - multi-user soft claims
  - polling
  - outcome updates
  - notes
  - export CSV
  - delete leads
  - reset called today
  - diagnostic chips
  - quality flag chips
  - call button copies number and opens tel link

## MMM Ad Studio
- Routes:
  `/ad-studio`
- Files:
  - `ad_routes.py`
  - `ad_generator.py`
- Purpose:
  Generate short-form social media ads/videos.
- Brands in `ad_generator.py`:
  - `junk_removal`
  - `landscaping`
- H&M ad brand:
  `H&M Junk Removal & Landscaping`
- H&M ad phone:
  `202-999-7885`
- H&M ad website:
  `hmjunkland.org`
- H&M service area:
  Herndon, Reston, Fairfax, Chantilly, nearby Northern Virginia.
- H&M voice:
  student-run, local, honest, neighborly, affordable, hardworking.
- Avoid words:
  `premier`, `elite`, `world-class`, `industry-leading`, `unmatched`, fake luxury language.
- CTA:
  `Text a picture for a free quote.`
- Ad studio supports:
  - photo uploads
  - demo photos
  - before/after layout
  - aspect ratios 9:16, 1:1, 16:9
  - music bed
  - local voiceover
  - captions
  - QR/tracked links
  - Facebook post support

## User Preferences / Working Style
- User types fast and rough.
- User wants Codex to act independently and not ask constant questions.
- User gets frustrated by repeated approval prompts.
- User wants visual work to be polished, not half-patched.
- User wants direct implementation, not long theory.
- User prefers:
  - local/trustworthy warmth
  - real visuals
  - bold but clean sections
  - strong spacing
  - less useless copy
  - no clutter
  - no fake claims
  - mobile must not break
- When visual feedback is given via screenshot:
  - treat it literally
  - patch the actual visible issue
  - do not keep adding hacks on top of hacks
  - if a component is bad, fully reset/remake it
- User likes concise summaries after work.
- User often wants prompts to send to another Codex/Claude/hive mind.

## Important Instruction For Future Codex
- Always inspect the actual screenshot and current code before patching.
- For H&M visual sections, prioritize the live page opened by file URL.
- Avoid absolute `/public/...` asset paths if the page is opened as `file:///`.
  Use relative:
  `public/assets/...`
- Do not use baked-in PNG text for readable website copy.
- Use PNGs as layers/backgrounds only unless the PNG is decorative or a mockup.
- For H&M phone area:
  Do not rebuild a fake 3D phone with CSS unless explicitly asked.
  Use the provided/remade PNG phone asset cleanly and align it to the shadow.
- If something looks janky, reset it instead of patching around it.
