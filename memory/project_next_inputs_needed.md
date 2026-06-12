---
name: Ad Studio — inputs to ask Miles for (reminder)
description: Prioritized list of real-world assets the user should provide to level up the Ad Studio output
type: project
---

Miles asked me to remind him later about what HE can provide to push the Ad Studio forward.
Bring this up next time he's ready to hand over assets. Ranked by impact:

1. **Real before/after job photos** (biggest unlock — everything currently runs on synthesized
   demo images). For auto before/after split layout, name pairs `before_*.jpg` / `after_*.jpg`,
   or just hand over before+after pairs and I'll wire naming.
2. **Logo file** (PNG, ideally transparent) — the overlay feature is now BUILT (2026-06-05) and
   dormant until a file is uploaded. Miles just uploads it via the navbar "🖼 Logo" button on
   /ad-studio (or drop a PNG at `ad_media/brand/logo.png`); it then burns into the top-right corner
   of every video automatically. No more code needed — just the asset.
3. **Real pricing / "starting at" numbers** (e.g. single item from $X, half-truck $X, mowing
   from $X) — so I can add an optional price overlay/caption. (NOTE: I may pre-build this price
   overlay feature ahead of time so it's ready when numbers arrive.)
4. **Facebook Page access token** — the "post → FB" button is built but needs the token to
   actually publish. Still $0, just a setup step; offer to walk him through getting it.
5. **2-3 real customer quotes/reviews** — rotate in as social-proof overlay frames.
6. **Free Pexels API key** (for the NEW Product Ads sub-tool at /ad-studio/products). Free signup
   at pexels.com/api, $0, commercial-use OK. Add `PEXELS_API_KEY=<key>` to .env and product ads
   start pulling real stock photos instead of drawn placeholder tiles. Dormant-until-keyed; works
   today with drawn tiles. (built 2026-06-08)

**Why:** the tool is functionally complete + verified at $0, so further quality gains now come
from real assets rather than more code. **How to apply:** when he says he has photos/logo/etc.,
process them and render a real, postable ad end-to-end (verify with an extracted frame).
My recommendation to him was: start with #1 (photos) + #2 (logo).
