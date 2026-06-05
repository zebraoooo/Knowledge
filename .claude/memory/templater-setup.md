---
name: templater-setup
description: Templater 配置细节和踩过的坑
metadata:
  type: feedback
---

**配置**：Templater 2.20.5，`Templates/Note.md` 作为全 vault 通用模板。

**关键配置项**（`.obsidian/plugins/templater-obsidian/data.json`）：

```json
{
  "templates_folder": "Templates",
  "enable_folder_templates": true,
  "trigger_on_file_creation": true,
  "auto_jump_to_cursor": true,
  "folder_templates": [{ "folder": "/", "template": "Templates/Note.md" }]
}
```

**踩过的坑**：

1. **改了 `data.json` 后 Obsidian 不会自动重读** → 必须重启应用或在插件列表里 disable→enable。写配置后必须提醒用户。

2. **`trigger_on_file_creation` 是 folder template 的真正总开关**，不是 `enable_folder_templates`。只开后者模板不会触发。`auto_jump_to_cursor` 也要一起开。

3. **Templater 只响应"真正创建文件"事件** — 文件列表新建、右键新建、`Ctrl+N` 都触发；快速切换器输入不存在文件名、图谱点击不存在的节点 → 不一定触发。

**Why:** 2026-06-05 配置时连续踩了前两个坑，用户测试发现"没有任何 frontmatter"，排查后修复。

**How to apply:** 给用户写 Templater 配置时，上述三项捆绑；写完必须提醒重启 Obsidian。
