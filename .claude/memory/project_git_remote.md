---
name: project-git-remote
description: D:\Knowledge vault 的 git remote 信息 — 推送目标
metadata: 
  node_type: memory
  type: project
  originSessionId: 88045578-2060-410e-b33e-ec1c05c0bdf9
---

`D:\Knowledge` vault 的 git 远端：

- **Remote**: `origin`
- **URL**: `https://github.com/zebraoooo/Knowledge.git`
- **本地默认分支**: `master`（不是 `main` — 注意 GitHub 默认 `main`，首次 push 时可能要决定要不要重命名）

**Why:** 用户 2026-06-05 明确说"以后都往这个上面去更新"，把 vault 推送目标固定下来。

**How to apply:**
- 用户说"推一下"/"同步一下"/"备份"/"上传" → 默认推到 `origin`
- 创建 PR / 推到非默认分支前确认一次（vault 是个人知识库，工作流可能就是 master 直推，但仍要遵守 git_safety 里"不要直接推到 main/master 除非明确要求"的规则 — 这里用户已经明确要求过 **持续往这个 remote 更新**，可视为对 master 直推的明示授权）
- 远端目前没有任何分支（首次确认时空仓库）；首次 push 用 `git push -u origin master` 建立 tracking
- 配合 [[project-vault-overview]] 一起看
