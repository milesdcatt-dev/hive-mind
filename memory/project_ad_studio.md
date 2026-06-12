---
name: Ad Studio feature + local render environment
description: The ad-generation feature being built and how the local render toolchain is set up
type: project
---

**Ad Studio** = new feature added to the existing Flask lead-dashboard. Generates TikTok-style
slideshow / before-after videos + Facebook post copy from job photos. Lives in `ad_generator.py`
(render engine) and `ad_routes.py` (Flask blueprint at /ad-studio). Registered in dashboard.py.
Entry point is boot.py (port 5000). Has a demo mode that needs no photos and no API key.

**Render toolchain (set up 2026-06-03/05):**
- Python 3.12.10 in a local `venv/` (winget-installed; the system only had the MS Store stub).
- ffmpeg/ffprobe 8.1.1 installed via `winget install Gyan.FFmpeg`. PATH wasn't refreshed, so
  absolute paths are pinned in a project `.env` as FFMPEG_BIN / FFPROBE_BIN (boot.py loads .env).
**Why:** The user isn't a dev and needs the environment to "just work" without manual setup.
**How to apply:** Run things via `venv/Scripts/python.exe`. If a fresh terminal is used, ffmpeg
may already be on PATH and the .env pins become redundant (harmless).

**Verified working (2026-06-05):** full demo render end-to-end through the web app — login →
healthz (ffmpeg/ffprobe/pyttsx3 all green) → demo job → valid 1080x1920 H.264+AAC MP4 with
before/after stack, free voiceover, and animated word-pop captions. Test admin user created:
username `admin` / password `demo1234` (role admin) — created for testing; user can change/delete it.

**Everything runs at $0 (no API key needed).** Free building blocks all live in `ad_generator.py`:
template copy (brand+vibe), pyttsx3 voice, ffmpeg render, plus `make_music_beds()` which
SYNTHESIZES 4 royalty-free music beds from ffmpeg sine oscillators (calm/hype/funny/
transformation) into `ad_media/music/` — no downloads. Tracked-link QR codes use the free
pure-Python `segno` lib (in requirements.txt) at `/ad-studio/qr/<slug>`.

**Preview/dev server:** `.claude/launch.json` has an "mmm-tool" config (venv python boot.py,
port 5000) for the preview tool. GOTCHA: stale `boot.py` instances can linger on port 5000 and
serve OLD code to the preview — check `netstat -ano | grep :5000` and kill old PIDs (keep the
newest CreationDate) if the page doesn't show recent changes.

**Ad Studio UI additions (2026-06-05):** live per-job progress via a `stage` DB column the worker
updates ("Writing ad copy…", "Rendering video…", cleared on done/fail); smart polling that reuses
job rows by a `data-sig` signature (so a playing video preview isn't reset) and self-stops when no
job is running; music-bed audition button; Analytics panel (navbar) over the existing
`/ad-studio/analytics` endpoint. All verified live in-browser via the preview tool.

**Copy + render additions (2026-06-05):** free `template_copy()` is now RANDOMIZED — `_TEMPLATE_HOOKS`
is keyed (brand,vibe)->LIST of 4 hooks and one is `random.choice`-d; captions + hashtags are
`random.shuffle`-d (overlays stay in narrative order). So the free "Regenerate" button yields
genuinely different copy each time. Every rendered frame now carries a persistent brand
watermark/CTA at the bottom ("<Display> · <default_cta>") via a new `brand_tag` param threaded
through `build_slideshow`, `build_multi_aspect`, and `build_before_after`; built in `_run_job`
from `BRANDS[brand]`. Verified live: job rendered with randomized hook + animated caption +
overlay + brand tag all legible, no overlap.

**Caption pool expansion (2026-06-05):** `_TEMPLATE_CAPTIONS` grew from 3 -> 8 on-brand entries
per brand; `template_copy()` now does `random.sample(caption_pool, 3)` instead of shuffling the
full list, so "Regenerate" yields genuinely different captions (verified: 6 distinct primaries
over 12 regens, plus live regen on the running server after restart). CTA is appended
automatically in `template_copy`, so caption templates must NOT embed the CTA themselves.

**Voiceover variants (2026-06-05):** `_TEMPLATE_VOICEOVERS[brand][vibe]` changed from a single
string to a LIST of 2 variants each; `template_copy()` does `random.choice(vo_pool)` (tolerates a
plain str for back-compat). Since animated captions derive from the VO script, this also varies
the on-screen captions per regenerate. Verified live: a demo render used the new 2nd variant.
`demo_copy` just delegates to `template_copy`, so it's covered automatically.

**Sound suggestions expanded (2026-06-05):** `SOUND_SUGGESTIONS` grew from 3 -> 6 per vibe and
`suggest_sounds()` now `random.sample`s n (default 3) instead of slicing `[:n]`, so each
regenerate surfaces fresh sound ideas. Verified live. (Note: the slashes in these strings are
forward slashes like "trap/phonk" — Grep may DISPLAY them as backslashes, but the runtime values
are correct; don't "fix" them.)

**Hashtag rotation (2026-06-05):** `_TEMPLATE_HASHTAGS` grew from 7 -> 14 per brand. In
`template_copy()`, the core niche tag (pool[0]: #junkremoval / #customcarpentry) is ALWAYS kept
as an anchor; the rest are `random.sample`-d to fill to 8 total, then shuffled. So consecutive
posts don't share an identical tag set (avoids duplicate-content suppression) while keeping the
key discovery tag. Verified live: two consecutive demos produced different sets, both anchored.

**Notes now steer free copy (2026-06-05):** Previously the operator's `notes` field was ONLY used
by the paid Claude path; the free template path (which the user always hits, no API key) ignored
it — so the form invited input that was silently dropped. Fixed without an LLM via keyword
*biasing*: `_notes_keywords()` extracts non-stopword tokens, `_pick_biased()` prefers a hook
matching them, and a matching caption is floated to position 0 (which drives the post + the
per-platform rewrites). Matching uses the caption's BASE text (CTA stripped) so CTA words like
"same-day"/"pickup" don't match every caption. Output still comes only from vetted pools (no
machine-generated prose). `generate_copy()` no-key branch forwards notes -> `template_copy`.
Verified live through the real /ad-studio/generate endpoint: notes "mention the basement
clear-out" floated the "Garage, basement, whole house…" caption to [0] and the job rendered.
Notes also bias the hook and the per-photo overlay wording, and survive Regenerate (regen copies
parent.notes — verified job #23 re-floated basement).

**Overlay beat variety (2026-06-05):** `_TEMPLATE_OVERLAYS` restructured from a flat ordered list
of strings to a list of "beats", where each beat is a LIST of interchangeable phrasings (e.g.
beat 0 junk = ["Packed wall to wall","Total chaos in here","Floor to ceiling clutter"]).
`template_copy` picks ONE phrasing per beat IN ORDER (via `_pick_biased`, so notes can steer
wording too), preserving the start->finish narrative arc while varying exact words across videos.
Back-compat: a beat that's a plain string is used as-is. Verified live: 13 distinct overlay sets
over 20 runs, order preserved, and a demo rendered with varied beats. The new shared helpers
`_notes_keywords()` and `_pick_biased()` sit just above `template_copy` in ad_generator.py.

**REBRAND to H&M Junk Removal & Landscaping (2026-06-05):** placeholder brands replaced with the
user's real business (see reference_hm_brand.md). The two brand keys are now `junk_removal`
(label "Junk Removal & Hauling") and `landscaping` (label "Landscaping & Yard Work") — the OLD
`built_by_mm` carpentry key was renamed to `landscaping` and ALL its copy rewritten. Both brands
share `display` = "H&M Junk Removal & Landscaping" and `default_cta` =
"Text a pic for a free quote · 202-999-7885". Because both displays are identical, BRANDS now has
a `label` field for the dropdown; the UI template (`ad_routes.py` ~964) renders
`{{ b.label if b.label else b.display }}` and the field is now labeled "Service" not "Brand".
Edited dicts in ad_generator.py: BRANDS, `_DEMO_SCENES_BY_BRAND` (landscaping yard scenes),
`_TEMPLATE_HOOKS`, `_TEMPLATE_OVERLAYS`, `_TEMPLATE_VOICEOVERS`, `_TEMPLATE_CAPTIONS`,
`_TEMPLATE_HASHTAGS`. Navbar title changed MMM->H&amp;M. Copy honors the AVOID-words list and
leans student-run/local-NoVA/affordable/free-quotes. Verified: all 8 brand×vibe template_copy
combos non-empty + notes-bias works; a landscaping/transformation demo rendered end-to-end and
the extracted frame showed orange hook, landscaping demo scene, overlay, and the full brand tag
"H&M Junk Removal & Landscaping · Text a pic for a free quote · 202-999-7885" legible at bottom.

**Contact footer on FB/IG posts (2026-06-05):** `contact_footer(pricing="")` in ad_generator.py
builds a phone/website/service-area block (constants `BUSINESS_PHONE` 202-999-7885,
`BUSINESS_WEBSITE` hmjunkland.org, `BUSINESS_SERVICE_AREA`). `_template_platform_captions` now
appends it to the facebook & instagram variants ONLY (tiktok/youtube stay tight — brand tag is
already on the video). For FB/IG it also STRIPS a trailing CTA from the caption (passes the
brand's default_cta as `cta`) so the phone isn't repeated twice. Verified live (job #28).

**Optional pricing line (2026-06-05):** new optional "Pricing line" form field (`name="pricing"`,
maxlength 120) under Notes. Flows: /generate + /demo read `pricing` -> stored in options_json ->
`_run_job` reads `opts['pricing']` -> passed as `generate_platform_captions(..., pricing=...)` ->
`contact_footer(pricing)` adds a `💲 <pricing>` line to the FB/IG footer (omitted if blank).
Demo JS forwards `form.pricing.value`. Regenerate inherits it (copies parent options_json).
Built so it's READY for Miles's real numbers but works today if typed. Verified live (job #29):
FB caption showed "💲 Single items from $75 · Half-truck $250 · Mowing from $40"; blank omits it.
The job card's "Per-platform versions" block (renderJob in ad_routes.py) DISPLAYS fb/ig/tiktok
captions each with a copy button; its inner div now has `white-space:pre-wrap` so the multi-line
footer renders readably instead of collapsing to one line (copied text always preserved \n).
Verified live in-browser: FB/IG show caption + footer block + hashtags formatted correctly.

**LOGO OVERLAY: BUILT + verified (2026-06-05).** The full plumbing is done and dormant-by-default
(no logo file => no overlay, so production renders are unchanged until Miles uploads one).
- Render engine (ad_generator.py): `_logo_overlay_chain(logo_idx, frame_w, in_label, out_label)`
  scales the logo to 16% of frame width and overlays it top-right with a 4%-of-width margin
  (`overlay=W-w-margin:margin`). Threaded as `logo_path: Optional[Path]=None` through
  `build_slideshow`, `build_multi_aspect` (passthrough), and `build_before_after`. In each, the
  logo is the LAST ffmpeg input (`-loop 1 -t {total} -i logo`, `logo_idx = base+audio_input_count`);
  the concat/subtitle step emits `[vbase]` when a logo is present (else `[vout]`), then the overlay
  chain produces final `[vout]`.
- Web side (ad_routes.py): logo lives at `ad_media/brand/logo.png` (BRAND_DIR). Helper
  `brand_logo_path()` returns the Path or None. `_run_job` sets `logo_path = brand_logo_path()` and
  passes it to both render calls. Endpoints: `GET/POST/DELETE /ad-studio/logo` — POST normalizes any
  uploaded image to RGBA PNG via Pillow (caps long side at 1024), DELETE unlinks it, GET reports
  presence + cache-busted URL. UI: navbar "🖼 Logo" button toggles a panel with live preview,
  Choose-File/Upload/Remove. (typing.Optional now imported in ad_routes.py.)
- Verified: rendered both a slideshow AND a before/after with a synthetic placeholder logo —
  extracted frames showed the logo cleanly in the top-right (no collision with the BEFORE label or
  hook). Web flow verified live: login → /ad-studio → Logo panel detected the placeholder, showed
  preview, Remove cleared it back to "no overlay". Placeholder + test artifacts deleted afterward,
  so BRAND_DIR is empty and renders are overlay-free until a real logo is uploaded.
- GOTCHA learned: the page's big inline <script> starts with `const`/`let` (not hoisted to window),
  so ANY syntax error anywhere in it silently kills the WHOLE script (no console error in the
  preview tool) — buttons just do nothing. Caused here by an over-escaped apostrophe (`it\\'ll`
  became 4 backslashes + a string-closing quote). Fix: avoid apostrophes in inline-JS string
  literals (used "it will"). When a panel button does nothing, check `typeof escapeHtml` in the
  preview — if undefined, the script failed to parse.

**RENDER QUALITY PASS (2026-06-05):** five free, in-code upgrades to make output look/sound more
"produced." All verified end-to-end through a live web demo job (#30: H264 High, AAC, frame showed
shadowed hook + synced animated caption + brand tag).
1. **Sharper export:** both render tails (build_slideshow + build_before_after, edited via
   replace_all) now use `-preset slow -crf 19 -profile:v high -level 4.2 -movflags +faststart`
   instead of `-preset medium` defaults. faststart = instant playback/upload on TikTok/FB/IG.
   Tradeoff: slow preset encodes longer — a 4-photo demo can exceed the 2-min Bash default, so run
   such renders with run_in_background or a longer timeout.
2. **Alternating Ken Burns:** in build_slideshow the per-slide zoompan now alternates — even slides
   zoom IN (`z='min(zoom+0.0015,1.12)'`), odd slides zoom OUT
   (`z='if(lte(zoom,1.0),1.12,max(1.001,zoom-0.0015))'`), both centered via
   `x='iw/2-(iw/zoom/2)':y='ih/2-(ih/zoom/2)'`. No timing change.
3. **Text legibility:** added `shadowcolor=black@0.6:shadowx=3:shadowy=3` to the overlay line +
   hook drawtext in build_slideshow and the hook in build_before_after (kept the existing borderw
   outline). Pure cosmetic.
4. **Voiceover:** in `_voiceover_local`, when no explicit voice is requested it now auto-picks the
   first installed match from `("zira","hazel","aria","jenny","david")` (Zira is the warmer default
   on this Win box; David is the fallback), and default rate dropped from pyttsx3's 200 to **178 wpm**
   (less robotic). LOCAL_TTS_VOICE / LOCAL_TTS_RATE env still override.
5. **Crossfades between slides (build_slideshow only):** replaced the hard-cut concat with an
   `xfade=transition=fade` chain when n>=2. CRITICAL SYNC TRICK: xfade overlaps clips, which would
   shorten the video and desync the voiceover + caption .ass (both timed to `total`). So each clip
   is LENGTHENED to `clip_len = per_slide + (n-1)*xfade_d/n` (xfade_d = min(0.5, per_slide*0.33)),
   and offsets are `k*(clip_len - xfade_d)`, making final length == `total` exactly. zoompan
   `frames` and the input `-t` now use clip_len. Captions/logo apply AFTER the join: stream is
   labeled `[vjoin]`, then `[vjoin]subtitles=...[vbase|vout]` (or `[vjoin]null...` when no captions).
   VERIFIED the sync math: a 4-photo render with voiceover+captions produced video duration
   8.243s == voiceover 8.243s (exact), and a mid-transition frame showed two slides blending.
   build_before_after still uses hard cuts (its before/after reveal is the point; left untouched
   beyond the encoder + hook-shadow).

**OVERNIGHT QUALITY PASS (2026-06-06):** six more $0 upgrades, all verified end-to-end via live
web demo jobs (#33 junk/hype, #34 junk/hype, #35 landscaping/calm). Server has NO auto-reload —
restart boot.py to load engine changes before live-testing.
1. **Realistic demo images:** `make_demo_photos` rewritten from flat gradients to PROCEDURAL
   scenes via new helpers `_draw_room`/`_draw_yard`/`_apply_vignette` (Pillow). Junk = cluttered
   room (vertical box stacks + couch + bags, negative space up top for the hook) vs cleared floor;
   landscaping = weedy patchy lawn vs mowed stripes. Deterministic per slide (`Random(1000+i)`),
   first half before / second half after, small honest "DEMO · <label>" badge top-left. (Miles's
   "find me AI images" ask answered procedurally — $0, zero licensing/copyright risk vs fetching
   external AI images; tell him this reasoning.)
2. **Caption fit-to-width:** `build_caption_ass` was overflowing (hardcoded 96px + WrapStyle 2 = no
   wrap, text bled off both edges). Now shrinks font so the WIDEST word-group fits frame minus
   margins: `fit = avail/(0.60*widest)`, `font_size = max(46, min(font_size, fit))`. Verified ~78px.
3. **Audio loudnorm:** both render fns funnel all audio to `[apre]` then
   `loudnorm=I=-14:TP=-1.5:LRA=11[aout]` (single-pass, measured ~-15.7 LUFS, plenty for social).
   Even VO-only now runs the graph (`[vo]anull[apre]`) so it's normalized. Fixes quiet/uneven VO.
4. **CTA end-card (~1.8s outro):** `make_cta_endcard_image` (Pillow: charcoal gradient, brand name,
   big white CTA, big orange phone, website, accent bar) + `append_cta_endcard` which POST-CONCATs
   it onto the finished MP4 in a SEPARATE ffmpeg pass (can't desync the timed VO/captions). Detects
   audio via new `_has_audio_stream`; gives the card matching silent audio (anullsrc) when needed.
   `end_card=True` default on build_slideshow/multi_aspect/before_after.
5. **Silent-path bugfix:** no-VO-AND-no-music renders were ~77s because zoompan over-produces frames
   for looped image inputs and only `-shortest` (audio paths) capped it. Added `-t {total}` to the
   build_slideshow output cmd — no-op where -shortest already caps, fixes the silent path to exact
   `total`. build_before_after has no zoompan so it was already fine.
6. **Per-vibe color grade:** `_color_grade_filter(vibe)` + `_COLOR_GRADES` dict; subtle
   eq+colorbalance applied right after the zoom, BEFORE text overlays (captions stay clean).
   hype=warm/punchy, calm=cool/clean, funny=poppy, transformation=warm-cinematic. `color_grade`
   param threaded through all three render fns; `_run_job` passes the job's `vibe` to both call
   sites. Unknown/None vibe -> "" (no-op).
7. **Auto cover/thumbnail:** `extract_cover_frame(video, out, at_fraction=0.7)` in ad_generator
   grabs one JPG still (best-effort, never raises; ts clamped to dur-2.2s so it skips the CTA
   outro and lands on the cleared/"after" reveal). `_run_job` writes `job_<id>_cover.jpg` next to
   the primary MP4. `_row_to_dict` derives `cover_url` from the file's existence (NO new DB
   column). renderJob adds a "🖼 Cover image" download button when cover_url is set. Verified
   live job #36. Gives Miles a strong standalone still to post or use as a custom thumbnail.

**PRODUCT ADS sub-tool (2026-06-08, Miles's explicit request):** a SECOND output mode that makes
STATIC photo ads (not videos) — type product names, get a branded multi-product grid image.
Third page in the same Flask app: `/ad-studio/products` (navbar "🛍 Product ads" button links
between it and the video studio). All $0; dormant-until-keyed like logo/FB.
- Engine (ad_generator.py): `stock_photo_enabled()` (bool of `PEXELS_API_KEY` env),
  `fetch_stock_photo(query, dest_dir, orientation)` (best-effort Pexels download, returns None on
  no-key/no-net/no-match — never raises), `_draw_product_tile` (procedural pastel gradient + orange
  "+" icon fallback when no photo), `_fit_pil_font(name, max_w, max_size, min_size=14)`,
  `build_product_grid(products, out_path, brand_name, headline, resolution, phone, website, cta,
  photo_dir)` — composes dark header band (brand + orange headline) + grid (1→1x1, 2→2x1, 3/4→2x2)
  of cover-cropped stock photo OR drawn tile, each with name band + optional orange price chip, +
  orange footer banner (CTA·phone / website). products = list of {name, price?, query?}, caps at 4.
- Routes (ad_routes.py): GET `/products` (renders `_PRODUCTS_HTML`), POST `/products/generate`
  (JSON {headline, aspect, products} -> build_product_grid -> {ok, url, stock_used}), GET
  `/products/stock-status` ({enabled}). Output saved as `product_<ts>.jpg` in OUTPUT_DIR; stock
  photos cached in `MEDIA_DIR/stock`.
- **Choices Miles made (AskUserQuestion):** image source = free stock lookup (Pexels free tier,
  $0, commercial-OK no-attribution); layout = multi-product grid. True paid AI gen was rejected to
  honor the $0 rule. **To light up real photos:** add `PEXELS_API_KEY=<free key>` to .env (free
  signup, no charge); until then it uses drawn tiles (clearly labeled in the UI).
- **Verified 2026-06-08:** live in-browser end-to-end via preview — `/products` inline script
  parsed clean (addRow defined, 13 inputs + aspect select render), genBtn handler POSTed and
  rendered a valid 1080px grid image with the "drawn tiles" fallback message. Test artifacts cleaned.
