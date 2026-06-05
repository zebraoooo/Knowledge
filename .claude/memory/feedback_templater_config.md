---
name: feedback-templater-config
description: 配置 Templater（及通用 Obsidian 插件）时容易踩的两个坑 — 文件改了不重载、folder template 的真正总开关
metadata: 
  node_type: memory
  type: feedback
  originSessionId: 88045578-2060-410e-b33e-ec1c05c0bdf9
---

配置 Obsidian 社区插件（特别是 Templater）的两条铁律：

## 1. 改了 `.obsidian/plugins/<id>/data.json` 必须让 Obsidian 重读

Obsidian 启动时把插件配置加载进内存后，**不会**主动监听 `data.json` 的文件变化。从命令行 / 编辑器改完 json，**Obsidian 看不到**。要么：

- 整个应用重启
- 在"第三方插件"里把这个插件 disable → enable

**Why:** 2026-06-05 配 Templater 时，第一次 push 完用户测试 "没有任何 frontmatter"，根因就是改了 data.json 但 Obsidian 还在用旧配置（其实那次的另一个原因是开关搞反了，见 §2，但即便开关对了不重载也不会生效）。

**How to apply:** 写完 `.obsidian/plugins/*/data.json` 之后，**必须**告诉用户重启 Obsidian 或 disable→enable 该插件。不要让用户自己发现"没生效"。

## 2. Templater 的 folder template 真正总开关是 `trigger_on_file_creation`

要让 "新建任意笔记自动套用模板" 这个体验生效，**两个**字段都必须开：

```json
{
  "enable_folder_templates": true,
  "trigger_on_file_creation": true,
  "folder_templates": [{ "folder": "/", "template": "Templates/Note.md" }]
}
```

只开 `enable_folder_templates` 是不够的 —— 这只是让"folder→template 映射表"被识别。`trigger_on_file_creation` 才是真正"创建文件这个事件触发模板"的开关。我第一次理解反了它的语义，导致用户新建笔记没反应。

`auto_jump_to_cursor: true` 一起开，模板里 `<% tp.file.cursor() %>` 才会真正把光标跳过去。

**How to apply:** 给用户写 Templater 配置时，这三项捆绑设置；不要再凭直觉判断 `trigger_on_file_creation` 该开还是该关。

## 3. Templater 的触发路径有局限（用户视角）

即便上述都配对，Templater 也只在 Obsidian 的**真正"创建文件"事件**上触发：
- 文件列表新建按钮、文件夹右键 "新建笔记"、`Ctrl+N` —— 都触发
- 从快速切换器输入新名、点关系图里不存在的节点、从其他笔记 `[[新名]]` 进去再确认创建 —— **不一定**触发，取决于 Obsidian 内部走的是哪条 API

**How to apply:** 用户报"模板没填"时，先确认他用什么方式建的笔记，再调配置。
