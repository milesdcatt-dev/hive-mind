---
name: Ad Studio — improvement backlog (ranked)
description: Living list of everything that could make the Ad Studio output better, ranked by impact vs effort
type: project
---

Miles asked (2026-06-06, overnight session) for "a list of everything that can be better."
This is the living backlog. Keep it updated as items ship. DONE items move to project_ad_studio.md.

## Shipped overnight 2026-06-06 (see project_ad_studio.md for detail)
- Realistic procedural demo images (drawn rooms/yards, not flat gradients)
- Animated-caption fit-to-width (no more text bleeding off both edges)
- Audio loudnorm to ~-14 LUFS (voiceover no longer quiet/uneven)
- CTA end-card (#6): ~1.8s branded outro (brand name + big CTA + orange phone +
  website + accent bar) appended via separate post-concat pass so it can't desync
  voiceover/captions. `end_card=True` default on build_slideshow/multi_aspect/
  before_after. Verified end-to-end via live web job #34.
- BONUS bugfix: silent-path (no voiceover AND no music) was producing ~77s videos
  because zoompan over-produces frames for looped inputs and only -shortest (audio
  paths) capped it. Added `-t {total}` to build_slideshow output — no-op for audio
  paths, fixes silent path. build_before_after unaffected (no zoompan).
- Per-vibe color grade (#12): `_color_grade_filter(vibe)` + `_COLOR_GRADES` dict in
  ad_generator. Subtle eq+colorbalance applied after zoom, before text overlays
  (captions stay clean). hype=warm/punchy, calm=cool/clean, funny=poppy,
  transformation=warm-cinematic. `color_grade` param on build_slideshow/
  multi_aspect/before_after; route passes the job's `vibe`. Verified live job #35
  (landscaping/calm) — full stack (grade+captions+VO+music+end-card) renders clean.
- Auto cover/thumbnail (#14): `extract_cover_frame(video, out, at_fraction=0.7)` in
  ad_generator (best-effort, never raises; backs off 2.2s from the tail so it skips
  the CTA outro and lands on the cleared/"after" reveal). `_run_job` writes
  `job_<id>_cover.jpg` next to the primary MP4; `_row_to_dict` derives `cover_url`
  (no DB column added); job card shows a "🖼 Cover image" download button. Verified
  live job #36 — cover was the swept-room reveal, button rendered with correct href.

## Shipped 2026-06-08 (see project_ad_studio.md for detail)
- **Product Ads sub-tool** (Miles's explicit request): static photo-ad generator at
  `/ad-studio/products` — type product names, get a branded multi-product grid image (not video).
  Free Pexels stock lookup (dormant until PEXELS_API_KEY added) + procedural drawn-tile fallback so
  it works today at $0. build_product_grid + fetch_stock_photo in ad_generator; /products routes +
  _PRODUCTS_HTML + "🛍 Product ads" navbar link in ad_routes. Verified live in-browser.

## HIGH IMPACT — needs an asset from Miles (no code can substitute)
1. **Real before/after job photos.** Still the #1 unlock. Demo images are now convincing but
   they're cartoons; real photos are what make a postable ad. Drop pairs in, name before_*/after_*.
2. **Logo PNG.** Overlay plumbing is BUILT + dormant; just upload via 🖼 Logo button.
3. **Real pricing numbers.** Pricing line field exists; real "from $X" numbers make it land.
4. **2-3 real customer reviews.** For a social-proof overlay frame (see #11 below).
5. **Facebook Page token.** "Post → FB" button is built; needs token to actually publish.

## MEDIUM IMPACT — pure code, free, can do unattended
6. **CTA end-card** (~1.5s closing frame: big phone + hmjunkland.org + "Text a pic for a free
   quote"). Best done via post-concat so it can't desync captions/voiceover. IN PROGRESS / next.
7. ~~**Whoosh/transition SFX**~~ DONE 2026-06-08. `make_transition_sfx(out_dir)` synthesizes a free
   0.5s pink-noise whoosh (ad_media/sfx/whoosh.mp3). `build_slideshow` accepts `transition_sfx_path`
   and lays a whoosh on EACH slide crossfade: splits the SFX into (n-1) copies, adelays each to the
   xfade center [k*(clip_len-xfade_d)+xfade_d/2], amix normalize=0 onto [apre] BEFORE loudnorm.
   Only fires when use_xfade AND there's already audio (silent path untouched); added before the
   logo input so logo_idx is unaffected. Threaded through build_multi_aspect; route synthesizes it
   (best-effort) + passes it. NOTE: only slideshow mode — before/after shows before+after in ONE
   frame (no cut to whoosh), so build_before_after intentionally has no SFX. Verified: music-only
   4-slide render, energy at the 3 transition centers was 2-13 dB louder than mid-slide baselines,
   and duration stayed EXACTLY 8.000s (no desync).
8. ~~**Better music beds.**~~ PARTIAL/DONE 2026-06-08. `make_music_beds` now layers a sub-octave
   bass (amix weight 0.55) under each chord for body + a gentle `chorus` for stereo thickness, so
   beds read as fuller pads not thin sine stacks. Regenerated all 4 with overwrite=True; verified
   stereo, correct durations (~41/45s), no clipping (max ~-11 to -13 dB, healthy headroom). Still
   no arp/percussion (rhythmic synthesis is much heavier); Miles can also drop real royalty-free
   MP3s in ad_media/music/ to fully replace these.
9. **Real speech-timed captions.** Captions are currently EVENLY distributed across duration (no
   speech alignment). Free offline Whisper / vosk could align words to the actual voiceover for
   perfect sync. Heavier dependency; evaluate.
10. **Neural TTS (Piper).** pyttsx3 (SAPI) is serviceable but still robotic. Piper is free + offline
    but needs a one-time voice-model download. Biggest audio "wow" if Miles wants it.

## POLISH — lower effort, nice-to-have
11. **Social-proof overlay frame** — rotate a real review as a styled quote card mid-video (#4).
12. ~~**Per-vibe color grade**~~ DONE 2026-06-06 (see Shipped section).
13. ~~**Hook A/B variants surfaced in UI**~~ DONE 2026-06-08. `hook_variants(brand,vibe,notes,n=3)`
    in ad_generator returns up to 3 DISTINCT hooks from the vetted `_TEMPLATE_HOOKS` pool, notes-
    matching ones floated to front (free/offline). New GET `/ad-studio/hooks?brand=&vibe=&notes=`
    returns them. UI: "✨ Suggest hooks" button on the generate form loads them as tappable orange
    chips; tapping sets hidden `hook_override` input (carried by the form's FormData). `_run_job`
    applies `opts['hook_override']` via `copy.hook = override` (only slide-1 hook; captions/VO
    untouched). Verified live: /hooks returned notes-biased options, in-browser the picker loaded 3
    chips, tap selected + set the hidden field correctly.
14. ~~**Auto thumbnail / cover frame** export~~ DONE 2026-06-06 (see Shipped section).
15. ~~**Aspect-aware caption position**~~ DONE 2026-06-08. `build_caption_ass` now picks the caption
    Y by aspect: portrait/9:16 stays 0.62 (verified-good), square/1:1 -> 0.60, landscape/16:9 ->
    0.55 (shorter frames, bottom is crowded by brand tag/hook). Verified: unit check of the \pos
    value for all 3 aspects + square burn-in render frame (caption clean, no brand-tag collision).
16. ~~**Saved presets**~~ DONE 2026-06-08. Presets bar atop the generate form (select + Load / 💾 Save
    current / Delete). Pure client-side localStorage (key `mmm_ad_presets`, $0, per-browser, no DB
    migration). Saves PRESET_FIELDS (brand/vibe/before_after/voice_provider/voice_name/
    animated_captions/music_track/seconds_per_photo) + aspects[]; photos/notes/hook stay job-
    specific (NOT saved). readForm/applyPreset/refreshPresetSelect helpers. NOTE: template is an
    inline Python string in ad_routes._PAGE_HTML, so HTML/JS edits need a SERVER RESTART to load
    (got me once this session). Verified live: save -> mutate form -> load restores every field
    exactly + appears in dropdown.
17. **Batch mode** — drop N photo pairs, get N ads in one go.

## TECH-DEBT / ROBUSTNESS
18. **Slow preset encode time** — `-preset slow` makes 4-photo renders sometimes exceed the 2-min
    Bash default. Fine in the web worker (threaded), but scripted test renders need bg/longer
    timeout. Could drop to `-preset medium` if encode time ever becomes a UX problem in-app.
19. **loudnorm is single-pass** (measured ~-15.7 vs -14 target). Two-pass would be exact but doubles
    audio analysis time; single-pass is plenty for social. Revisit only if levels feel off.
20. **No automated test for render pipeline** — every change is verified by hand via extracted
    frames. A tiny smoke-test script (render 1 demo, assert MP4 valid + has audio) would catch
    regressions fast.
