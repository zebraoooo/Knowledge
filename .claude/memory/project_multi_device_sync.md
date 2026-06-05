---
name: project-multi-device-sync
description: "用户在多台电脑上使用同一个 vault — 通过 git remote 同步，需要按\"per-device vs shared\"区分配置"
metadata: 
  node_type: memory
  type: project
  originSessionId: 88045578-2060-410e-b33e-ec1c05c0bdf9
---

用户会在**多台电脑**上使用 `D:\Knowledge` 这个 Obsidian vault，通过 [[project-git-remote]] 提到的 GitHub remote 跨设备同步。

**Why:** 用户 2026-06-05 主动告知"以后可能不只一个电脑去使用这个知识库"，这直接影响 `.gitignore` 策略和未来对 `.obsidian/` 配置的处理方式。

**How to apply:**

1. **`.gitignore` 必须区分两类文件**：
   - **per-device（忽略）**：`workspace.json`（UI 布局）、`cache/`、`file-recovery/`、`plugins/*/data.json`（插件本地数据，常含 token / 最后同步时间）、`.claude/settings.local.json`
   - **shared（追踪）**：`app.json`、`appearance.json`、`core-plugins.json`、`community-plugins.json`、`hotkeys.json`、`snippets/`、插件本体（`plugins/*/main.js` `manifest.json` `styles.css`）

2. **Obsidian Sync 与 git 并存**：vault 已启用核心插件 `sync`（Obsidian 官方付费同步）。如果用户实际同时用 Obsidian Sync + git，两者可能冲突；遇到同步异常时先确认是哪条链路出了问题。

3. **冲突优先 Markdown 笔记内容**：跨设备最容易冲突的是 `workspace.json` 这类 UI 状态（已被 ignore）和正在编辑的同名笔记。引导用户在切换设备前先 commit & push。

4. **首次推送注意**：如果用户在另一台设备上也想同步配置，他需要先在那台机器 `git clone` 后再装 Obsidian 打开 vault — 而不是反过来（先建 vault 再 clone 会冲突）。

5. **新增插件数据时**：装新社区插件后，检查 `.obsidian/plugins/<name>/data.json` 里有没有 token / API key / 设备特定路径，必要时补到 `.gitignore`。
