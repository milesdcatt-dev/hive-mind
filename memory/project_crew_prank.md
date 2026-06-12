---
name: crew.jpg chubby-prank + revert backup
description: Left crew member on hmjunkland-site-v2 crew.jpg was warped chubbier/shorter as a prank; pristine original saved for revert
type: project
---

On 2026-06-10 Miles asked to prank the LEFT crew member in
`Desktop/hmjunkland-site-v2/assets/crew.jpg` (3 guys + truck) — make him a bit
chubbier and shorter, "noticeable but not too fat."

**Revert (do this when he asks to undo the prank):** the pristine original is saved
at `assets/crew_original.jpg`. Restore with `cp assets/crew_original.jpg assets/crew.jpg`.

**Why / How to apply:** done with a pure-PIL feathered MESH warp (no AI tools
available; numpy/scipy absent). Script is `assets/_prank_warp.py` — it always warps
FROM crew_original.jpg, so re-running is safe and the effect can be re-tuned via
FATTEN/SHORTEN constants. Warp is gated to the left guy's column (CX=316, FEET=965)
so the other two crew + truck stay untouched.
