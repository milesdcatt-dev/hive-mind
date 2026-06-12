---
name: No paid APIs; work autonomously
description: Hard constraint to avoid spending money + preference for long autonomous work sessions
type: feedback
---

**Rule 1 — Never incur cost / use paid APIs unless explicitly told.**
**Why:** User said "fuck eleven labs for now" and later "do not go over usag or spednd moeny ofc."
They want $0 solutions.
**How to apply:** Default every feature to free/offline options. Voice = pyttsx3 (free Windows
SAPI5), not ElevenLabs. ElevenLabs/paid voice only if a key is already present. Copy generation
needs ANTHROPIC_API_KEY but must always fall back to free demo/canned copy when absent — never
require a paid key. Don't sign up for, configure, or call anything that bills.

**Rule 2 — Work autonomously for long stretches; don't stop to ask.**
**Why:** Repeatedly said "keep workign on it non stop use all tokens", "do what u think is best
i giv eu full accesse and just keep makign good changes."
**How to apply:** When given an autonomy grant, keep making good, detailed, *verified* changes
without pausing for approval on each step. Be thorough, verify each change works, prefer real
fixes over flashy half-features. Still avoid destructive/irreversible actions without thought.
