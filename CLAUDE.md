# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository nature

This is **not a code project** — it is the user's personal **Obsidian vault** (`D:\Knowledge`). Treat tasks here as knowledge-management work (Markdown notes, frontmatter, `[[wikilinks]]`, tags, canvases), not software engineering.

There is no build system, no test runner, no package manager. Do not invent or run commands like `npm`, `pnpm`, `pytest`, etc. — they don't apply here.

## Current state

- Templater 2.20.5 installed and configured — `Templates/Note.md` auto-applies to new notes vault-wide
- `Templates/` holds universal note skeleton (title, created, updated, source, status, type, tags, aliases in frontmatter)
- Git remote: `origin` → `https://github.com/zebraoooo/Knowledge.git` (master), user authorizes direct push
- `.gitignore` separates per-device state from shared config; `.obsidian/workspace.json`, plugin `data.json` (except Templater's), `.trash/` are ignored
- Core plugins enabled include: file-explorer, search, graph, backlink, canvas, daily-notes, templates, bases, **sync**

Because Obsidian Sync is enabled, changes under `.obsidian/` propagate to the user's other devices — confirm before modifying core/community plugin config or workspace layout.

## Working conventions

- **File creation**: prefer Markdown with YAML frontmatter when metadata is useful. Use `[[wikilinks]]` for cross-note references and `#tags` (no spaces) for taxonomy. Avoid Windows-illegal filename characters (`\ / : * ? " < > |`).
- **Editing existing notes**: keep the user's tone, heading style, and link conventions. Don't reformat aggressively.
- **Don't auto-create scaffolding**: no PARA folders, no Templates folder, no MOCs unless the user asks. The vault is intentionally bare right now.
- **Re-check structure each session**: this file's "current state" snapshot will go stale as the vault grows. Run a quick `ls` / Glob before assuming the layout.

## What lives where

- `.obsidian/` — Obsidian app config (tracked in git). Workspace layout, enabled plugins, appearance.
- `.claude/` — Claude Code project-local settings (settings.local.json, skills, agents). **Also contains vault-specific memory at `.claude/memory/MEMORY.md`** — load this index early in each session. It holds vault-level project/feedback context distinct from the harness-wide memory at `%USERPROFILE%\.claude\projects\D--Knowledge\memory\`.
- `.git/` — git repo.
- `Templates/` — Templater note templates.

## Out of scope

Do not generate sections about build/lint/test, architecture diagrams, or "common development tasks" — none exist for a notes vault. If the user later adds Templater scripts, Dataview queries, or custom CSS snippets that warrant explanation, update this file then.
