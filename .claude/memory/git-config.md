---
name: git-config
description: vault 的 git remote 和推送约定
metadata:
  type: project
---

- **Remote**: `origin` → `https://github.com/zebraoooo/Knowledge.git`
- **默认分支**: `master`
- **推送策略**: 用户已授权对 master 直推（2026-06-05 明确说"以后都往这个上面去更新"）

**How to apply:**
- 用户说"推送/同步/备份/上传" → 推到 origin master
- 提交前检查 `.gitignore` 没漏掉不该追踪的文件
- 首次推送 `-u origin master` 已建立 tracking
