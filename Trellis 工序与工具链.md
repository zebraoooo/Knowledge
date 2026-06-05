---
title: Trellis 驱动下的工序安排与工具链
created: 2026-06-05
updated: 2026-06-05
source: https://linux.do/t/topic/2100914
status: 草稿
type: 技术
tags: [trellis, ai, dev-workflow, mcp, tools, claude-code, codex]
aliases: []
---

# Trellis 驱动下的工序安排与工具链

来自 linux.do 社区的一篇实战总结帖。

## 对 Trellis 的评价

作者认为 Trellis 是目前最好的 agent harness 框架：

- **轻盈**：指令集不冗余，新手友好
- **完善**：项目开发每个阶段都有覆盖，hook 控制做得好，对市面上主流 AI 编程工具都丝滑兼容
- **独特**：对非编程场景（如写作）也兼容，引入了 monorepo 和模板技术，支持素材、版本、任务控制

## 推荐的开发工序

1. 创建项目文件夹，里面建 `docs/`（文档）和 `prompts/`（提示词 + 记录开发阶段）
2. 使用 `/trellis:brainstorm` 描述需求
3. 确定需求后用 `update-spec` 生成 100-500 个问题的需求表（按自己的判断决定数量）
4. **双模型校验**：先让 Opus 4.7 跑一轮 spec/prd，再让 GPT-5.5 补充一轮，确保完善
5. 需求表填写完后，再次用 Opus 4.7 + `update-spec` 将需求解耦成 PRD 和 Spec
6. 确定 PRD 和 Spec 后，**驱动 GPT-5.5 开发**（作者认为 GPT-5.5 自纠错能力比 Opus 4.7 强，"爆杀"）
7. 开发完人工逐项核对 PRD/Spec，防止漏项（GPT-5.5 仍有幻觉，"没开发但填已完成"）
8. 长期任务：搭配 `codex-autoresearch` skill，让它读 `prompts/` 下的子文件夹自动推进

## 关键提醒

- **Claude Code 用户必须开启 ToolSearch 功能**，节省 token 和上下文
- 工具不是越多越好，做好工具箱管理：用多少开多少，节省资源和上下文
- 很多 MCP 工具三天两头出问题，每次开始工作前先扫一遍环境确认工具正常

## 推荐的工具链

### 通用

| 名称                                | 类型  | 说明                                                |
| --------------------------------- | --- | ------------------------------------------------- |
| Grok-Search with Tavily/Firecrawl | MCP | 即便算力不能白嫖，tavily 和 firecrawl 的兼容弥补了这一点，作者认为最好的搜索工具 |
| memory                            | MCP | 记忆管理，`@modelcontextprotocol/server-memory`        |
| sequential-thinking               | MCP | "金字招牌"，任何时候都不会过时                                  |
| markitdown                        | MCP | 微软出品，最好的 Markdown 转换工具，需本地部署                      |
| context7                          | MCP | 获取最新项目/语言/网页信息，但 AI 很少主动用                         |
| deepwiki                          | MCP | 跟 context7 同作用，杜绝幻觉提升准确性                          |
| playwright                        | MCP | 泛用性极高，不仅能开发还能做浏览器操作                               |
| Grok2API                          | 纯项目 | Grok 集中管理网关                                       |

### 软件开发专用

| 名称              | 类型  | 说明                                     |
| --------------- | --- | -------------------------------------- |
| exa             | MCP | 搜索 + 编程，需付费但有注册机                       |
| gitnexus        | MCP | 零服务器代码知识图谱引擎，需本地部署和初始化                 |
| serena          | MCP | 强大的语义检索和编辑工具包，"Codex 强烈推荐"，准确度高了很多     |
| mobile-mcp      | MCP | 安卓模拟器搭配有奇效，但不稳定                        |
| shadcn          | MCP | React/Vue 前端组件库，组件齐全、规范                |
| chrome-devtools | MCP | 进阶需求（性能测试、逆向），搭配 playwright，token 消耗大户 |
| codex-cli       | MCP | Codex 原生 MCP，也可反向装给 Claude             |
| dart            | MCP | 谷歌官方 Dart/Flutter 工具，需本地部署             |

## 三句话总结

1. Trellis 做工序编排（brainstorm → update-spec → 开发 → 手测核验），Opus 4.7 做 spec 生成，GPT-5.5 做实际开发
2. 工具箱重要但更要管理好：用多少开多少，开工前扫一遍环境
3. 人做最后的兜底，逐项核对 PRD/Spec 表格

## 相关

- [[Trellis]] — 规范驱动开发框架
- [[Trellis + GitNexus + Serena 工作流]] — 三件套组合总结
- [[Serena]] — IDE 级语义代码操作

## 来源

- https://linux.do/t/topic/2100914
- Trellis: https://github.com/mindfold-ai/Trellis
- codex-autoresearch: https://github.com/leo-lilinxiao/codex-autoresearch
