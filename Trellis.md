---
title: Trellis
created: 2026-06-05
updated: 2026-06-05
source: https://trellis-lake.vercel.app/
status: draft
type: tech
tags: [ai, dev-tools, spec-driven, claude-code]
aliases: []
---

# Trellis

AI 驱动的开发协作与编码框架。一句话：**Trellis 是 AI 的脚手架** —— 通过自动化机制注入项目规范，引导 AI 沿着规范的路径前进。

作者是 Junsen Huang（GitHub: huangjunsen0406），也是 py-xiaozhi、UnifyPy、xiaozhi-mcphub 等项目的作者。npm 包 `@mindfoldhq/trellis`，GitHub 8.6k stars。

## 解决了什么问题

AI 编码三个核心痛点：

1. **Vibe Coding**：AI 随机发挥，同一个需求三次生成三种写法，质量看运气
2. **上下文丢失**：每次新会话 AI 都"失忆"，反复解释项目约定
3. **规范碎片化**：项目规范散落在 PR 评论、Slack 和老同事记忆里，AI 无从得知

核心观点：**代码不再是资产，规范才是资产**。

## 四个核心概念

1. **Spec（规范）**—— `.trellis/spec/`，Markdown 写的编码标准，按模块分文件（frontend / backend / guides），AI 写代码前先读规范

2. **Workspace（工作区）**—— `.trellis/workspace/`，每个开发者的会话日志（Journal），让 AI 跨会话记住上次做了什么

3. **Task（任务）**—— `.trellis/tasks/`，工作单元，含 PRD、上下文配置、子任务。完整生命周期：创建 → 规划 → 执行 → 验证 → 归档

4. **Skill（技能）**—— `.agents/skills/`，Auto-trigger 工作流模块，平台根据上下文自动触发

## 安装与使用

要求 Node.js 18+ 和 Python 3.9+，Mac / Linux / Windows 全支持。

```bash
npm install -g @mindfoldhq/trellis
trellis init -u your-name          # 初始化
trellis init -u your-name --claude --cursor --windsurf  # 指定平台
trellis init -u your-name --template electron-fullstack   # 远程模板
trellis init --registry gh:myorg/myrepo/specs            # 团队 Registry
```

## 日常三个命令

| 命令 | 作用 |
|---|---|
| `/trellis:start` | 开启会话，读取工作流契约、身份、git 状态、活跃任务、Spec 索引 |
| `/trellis:continue` | 推进下一步，AI 自动判断当前阶段（brainstorm → implement → check） |
| `/trellis:finish-work` | 收尾归档，前提是代码已 commit |

典型流程：描述需求 → AI brainstorm → `/trellis:continue` → 实现 → `/trellis:continue` → 检查 → `/trellis:finish-work`

## Auto-trigger Skills

五个自动触发的工作流，不需要手动调：

- **trellis-brainstorm**：用户描述需求时触发，一问一答澄清需求，起草 PRD
- **trellis-before-dev**：改代码前触发，读取受影响模块的 spec，确保 AI 了解约定
- **trellis-check**：实现完成后触发，对照 spec 审查 diff，运行 lint/typecheck/test
- **trellis-update-spec**：有值得沉淀的知识时触发，固化为 decision / convention / pattern / gotcha
- **trellis-break-loop**：修完棘手 bug 后触发，五维根因分析（分类 → 失败原因 → 预防机制 → 扩散检查 → 知识固化）

## 横向对比

| 维度 | OpenSpec (51.3k★) | Superpowers (210.2k★) | Trellis (8.6k★) |
|---|---|---|---|
| 规范管理 | Spec 库 | 无 | Spec 分层 + 自更新 |
| 工程技能 | 手动命令 | TDD/规划/审查 | Auto-trigger Skill |
| 跨会话记忆 | Spec 文档持久化 | 无 | Spec + Workspace Journal |
| 任务管理 | 四阶段流程 | 无 | 完整生命周期 + Hook |
| Sub-agent | 无 | 子代理开发 | Research/Implement/Check |
| 团队共享 | Git + delta | 通用 Skill 共享 | 项目专属 Spec + Registry |
| 平台支持 | 20+ 工具 | 6+ 工具 | 14+ 深度集成 |
| 代码结构感知 | 无 | 无 | 可配合 GitNexus |

官方推荐组合：**Trellis（规范 + 工作流）+ GitNexus（代码结构感知）= 完整 AI 工程方案**

## 七大业务场景

1. 从零开始新项目（B2B Dashboard，不再每次重新发明目录结构和 API 风格）
2. 接入存量项目（三年老 SaaS 仓库，从真实代码提取隐含模式到 Spec）
3. 交付产品功能（团队邀请，横跨权限、API、数据、UI、邮件五层）
4. 重构老模块（1200 行发票服务，定义不变量后逐层安全拆分）
5. 反复出现的 Bug（Python 版本 PATH 陷阱，根因 → 修复 → 预防闭环）
6. 减少重复 Review（Review 模式映射为 Spec 规则，AI 提交前自动检查）
7. 推广到 50 人团队（多 AI 工具共享同一套规范，不强制统一 IDE）

## 团队推广（50 人规模）

分五个阶段推进，目前文档未展开细节。核心思路：从个人到团队逐步采用，不强制统一 IDE，靠 Git 版本化的 Spec 库实现共享。

## 支持的平台

Claude Code, Cursor, OpenCode, Codex, Gemini CLI, Windsurf, Kiro, Copilot 等 14+ 平台。

## 相关

- GitNexus：零服务器代码知识图谱引擎，Tree-sitter AST + Graph RAG，40.5k stars
- OpenSpec：轻量级 Spec 驱动开发框架，四阶段工作流（Propose → Review → Apply → Archive）
- Superpowers：Agentic Skills 框架，给 AI 注入可组合工程技能

## 来源

- 官网（Slidev 演示）：https://trellis-lake.vercel.app/
- npm：`@mindfoldhq/trellis`
- 作者 GitHub：https://github.com/huangjunsen0406
