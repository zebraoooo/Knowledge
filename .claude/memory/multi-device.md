---
name: multi-device
description: 多设备使用策略 — .gitignore 区分 per-device 与 shared
metadata:
  type: project
---

用户多台电脑使用此 vault，通过 git 同步。

**.gitignore 策略**：

| 忽略（per-device） | 追踪（shared） |
|---|---|
| `workspace.json` | `app.json` |
| `cache/` | `appearance.json` |
| `file-recovery/` | `core-plugins.json` |
| `.trash/` | `community-plugins.json` |
| `plugins/*/data.json`（除 Templater） | `hotkeys.json` |
| `.claude/settings.local.json` | 插件本体代码 |
| OS 垃圾文件 | `snippets/` |

Templater 的 `data.json` 例外追踪（已验证无 token/绝对路径）。

**新机器首次使用**: 先 `git clone`，再用 Obsidian 打开此目录。Obsidian Sync 核心插件当前也开着，注意两条同步链路可能冲突。

**How to apply:**
- 新增插件时检查其 `data.json` 有无敏感信息，决定是否例外追踪
- 用户报同步冲突时先确认是哪条链路出的问题
