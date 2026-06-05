---
name: vault-overview
description: D:\Knowledge 是个人 Obsidian 知识库 — 定位、当前状态、关键配置
metadata:
  type: project
---

`D:\Knowledge` 是个人 Obsidian vault，不是代码项目。

**当前状态（2026-06-05）**：
- 已装 Templater 2.20.5，`Templates/Note.md` 作为全 vault 通用模板，新建笔记自动套用
- git remote `origin` → `https://github.com/zebraoooo/Knowledge.git`（branch `master`），用户授权直推
- `.gitignore` 已区分 per-device 与 shared

**Obsidian 配置**：
- 核心插件：file-explorer, search, graph, backlink, canvas, daily-notes, templates, bases, sync 等
- 社区插件：Templater（SilentVoid，v2.20.5）
- 外观：默认主题，无自定义 CSS

**How to apply:**
- 笔记 / 知识管理为主，不是写代码
- Markdown + frontmatter + `[[wikilinks]]` + `#tags`
- 组织方法（PARA/Zettelkasten/MOC）先问再动
- 改 `.obsidian/` 配置前确认（sync 已开启）
- 每次会话先 `ls` 看结构，不依赖此文件的时间快照
