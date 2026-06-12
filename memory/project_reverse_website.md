---
name: Inson reverse-website project
description: Separate Inson designer portfolio site Miles is building in Codex; lives on Desktop, pushed to milesdcatt-dev/reverse-website
type: project
---

Miles is building an "Inson" designer/architect portfolio website (the "reverse website") at `C:\Users\Miles Dickey\Desktop\reverse-website` — a SEPARATE repo from mmm-tool-v1. Remote: `github.com/milesdcatt-dev/reverse-website` (main branch). Static HTML site: index + inson-biography/practice/projects/publications/reviews pages, ~30 project detail pages in projects/, ~450 gallery images under assets/inson/.

**Why:** Primary build happens in **Codex** (OpenAI's agent), not Claude Code. Miles occasionally asks Claude Code to push when Codex is "out"/down.

**How to apply:** When Miles refers to "the reverse website" or "inson's website," it's this Desktop repo, not the working dir. Because Codex builds it in parallel, Claude pushes can leave Codex's local main behind origin — remind Miles to have Codex `git pull` before its next commit. Added a .gitignore (excludes .claude/, .preview-server.*.log) on 2026-06-10.

**CRITICAL — the live file is misleadingly named (confirmed 2026-06-10):** The actively-built homepage is `index.backup.2026-05-26.html` (~436KB, freshly modified, has the full banner / 30-residences / archive runway). The file actually named `index.html` is the OLD/short version (~128KB, Jun 9, different structure: `.projects-toolbar`/`.projects-grid`). The browser loads `index.html` at `/`, but Miles views and Codex builds the "backup"-named file. ALWAYS edit `index.backup.2026-05-26.html` for live changes; do NOT trust the `index.html` name. Edits there are uncommitted and unstaged in git. Open question for Miles: whether to promote the big file to real `index.html` and stop the "backup" misnomer.

**Codex/Claude same-file collision risk:** On 2026-06-10 Miles worried he'd made Codex and Claude push & edit the same file. Git was clean and in sync with origin (not ahead/behind) — no actual collision had happened — but both tools DO write to `index.backup.2026-05-26.html`. Before editing, confirm whether Codex is actively running so the two agents don't overwrite each other.

**Banner styling done 2026-06-10 (Claude):** Appended a "Step 24" CSS block before `</style>` making the "View all 30 projects" CTA (`.project-deep-link::before`) mahogany (`#6b3a24→#3c1d12`, was crimson `#9b2530`), softening the over-bright top-left radial glow, and centering the eyebrow/title/`small` text (title was left-aligned). Verified live via preview_eval on port 5173.

**Preview gotcha:** `preview_screenshot` fails in this env ("display surface not available"); verify visual/CSS changes with `preview_eval` / `preview_inspect` instead. The reverse-website preview config (port 5173, `--directory` to the Desktop repo) was added to mmm-tool-v1's `.claude/launch.json` on 2026-06-10.
