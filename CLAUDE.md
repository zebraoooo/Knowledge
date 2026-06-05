# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository nature

This is **not a code project** — it is the user's personal **Obsidian vault** (`D:\Knowledge`). Treat tasks here as knowledge-management work (Markdown notes, frontmatter, `[[wikilinks]]`, tags, canvases), not software engineering.

There is no build system, no test runner, no package manager. Do not invent or run commands like `npm`, `pnpm`, `pytest`, etc. — they don't apply here.

## Current state

The vault is essentially empty as of initial setup:

- No `.md` notes yet at the top level
- Git is initialized (`master` branch) but has no commits
- `.obsidian/` holds default configuration only — no community plugins installed (`community-plugins.json` is `[]`)
- Core plugins enabled include: file-explorer, search, graph, backlink, canvas, daily-notes, templates, bases, **sync**

Because Obsidian Sync is enabled, changes under `.obsidian/` propagate to the user's other devices — confirm before modifying core/community plugin config or workspace layout.

## Working conventions

- **File creation**: prefer Markdown with YAML frontmatter when metadata is useful. Use `[[wikilinks]]` for cross-note references and `#tags` (no spaces) for taxonomy. Avoid Windows-illegal filename characters (`\ / : * ? " < > |`).
- **Editing existing notes**: keep the user's tone, heading style, and link conventions. Don't reformat aggressively.
- **Don't auto-create scaffolding**: no PARA folders, no Templates folder, no MOCs unless the user asks. The vault is intentionally bare right now.
- **Re-check structure each session**: this file's "current state" snapshot will go stale as the vault grows. Run a quick `ls` / Glob before assuming the layout.

## What lives where

- `.obsidian/` — Obsidian app config (tracked in git). Workspace layout, enabled plugins, appearance.
- `.claude/` — Claude Code project-local settings.
- `.git/` — git repo (currently no commits, no remote configured here).

## Out of scope

Do not generate sections about build/lint/test, architecture diagrams, or "common development tasks" — none exist for a notes vault. If the user later adds Templater scripts, Dataview queries, or custom CSS snippets that warrant explanation, update this file then.
