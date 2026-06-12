# Hive Mind — Miles' Shared Knowledge Base

Central context store so Claude, Codex, and ChatGPT all work from the same memory.

> KEEP THIS REPOSITORY PRIVATE. `context/full_context_dump_PERSONAL.md` contains
> personal details (relationships, health, beliefs, finances).

## Layout

### `memory/` — Claude auto-memory (live, structured)
Exported from Claude Code's auto-memory system. The index is `memory/MEMORY.md`.
- `user_profile.md`, `reference_hm_brand.md`
- feedback notes (free tools/autonomy, site colors)
- project notes (ad studio, hm website, hm loader, improvement backlog, next inputs, reverse website, crew prank)

### `context/` — long-form context dumps
- `claude_project_memory.md` — Claude's full project-memory dump (H&M, BuiltByM, MMM tool/dashboard, ad studio, working style)
- `chatgpt_version_context.md` — the ChatGPT-built context version (brand identity, design preferences, copy, asset ideas across H&M / Built By M / Inson)
- `full_context_dump_PERSONAL.md` — comprehensive operator profile (ventures, school, creative projects, personal). SENSITIVE.

## Notes on sources
- Codex on-disk memory store (`~/.codex/memories`) was empty at export time — nothing to add.
- ChatGPT memory is stored server-side in OpenAI's cloud and cannot be exported from disk; `chatgpt_version_context.md` is the manually-maintained equivalent.
- These are point-in-time snapshots. Re-export when memory changes.
