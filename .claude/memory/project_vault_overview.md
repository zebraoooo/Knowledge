---
name: project-vault-overview
description: D:\Knowledge is the user's personal Obsidian vault — purpose, structure, current state, and key config choices
metadata:
  type: project
---

`D:\Knowledge` 是用户的个人 Obsidian 知识库 vault（不是代码项目）。

**当前状态（2026-06-05）**：
- 已装 Templater 2.20.5，配置了 `Templates/Note.md` 作为全 vault 通用模板
- git remote `origin` → `https://github.com/zebraoooo/Knowledge.git`（branch `master`）
- `.gitignore` 已配置，区分 per-device（workspace.json、plugin data.json、cache、trash）和 shared（核心配置、插件代码、Templater 的 data.json）
- 记忆系统已移到 `.claude/memory/`，跟随 git 跨设备同步

**Obsidian 配置**：
- 核心插件启用：file-explorer, search, graph, backlink, canvas, daily-notes, templates, bases, **sync** 等
- 已装社区插件：Templater（SilentVoid，v2.20.5）
- `app.json` / `appearance.json` 都是默认空 `{}`，没有自定义主题或设置

**Why:** 用户在初次会话中明确告知"当前目录是 obsidian 文件夹，作为我的个人知识库"。

**How to apply:**
- 这里的工作以**笔记 / 知识管理**为主，不是写代码
- 默认按 Markdown 笔记、frontmatter、`[[wikilinks]]`、tags、Obsidian 语法处理
- 需要 PARA、Zettelkasten、MOC 等组织方法时，先问用户偏好再动手
- `.obsidian/sync` 已开启 — 修改核心配置前先确认
- vault 内容会随时间增长，未来读取时**先 ls 一遍**再判断结构，不要依赖这条记忆里的快照
