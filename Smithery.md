---
title: Smithery
created: 2026-06-05
updated: 2026-06-05
source:
  - https://smithery.ai
  - https://github.com/smithery-ai
status: 草稿
type: 技术
tags: [mcp, ai, tools, marketplace, claude-code]
aliases: [Smithery.ai, MCP 应用商店]
---

# Smithery

MCP 生态的应用商店。6000+ 社区 MCP Server 的中央市场，搜 → 找 → 装一条龙，省去手动翻 GitHub、抄配置的过程。

类比：**Smithery 之于 MCP Server，等于 npm 之于 JS 包、Docker Hub 之于容器镜像。**

## 解决什么问题

在没有注册中心之前，装一个 MCP Server 的流程是：

1. 听说某个工具好用（社区帖子、口口相传）
2. 去 GitHub 找仓库
3. 看 README 抄 MCP 配置
4. 手动写进 `~/.claude.json`
5. 重启 Claude Code 看能不能连上

Smithery 把它变成：搜索 → 点安装 → 完事。

## 核心功能

| 功能 | 说明 |
| ---- | ---- |
| 搜索发现 | 按名称、标签搜索，支持自然语言描述搜 |
| 一键安装 | 自动写入 Claude Code / Cursor / Windsurf 等客户端的 MCP 配置 |
| 发布 | MCP Server 作者上传到市场，支持 stdio 和 HTTP 两种传输方式 |
| 连接管理 | 集中管理 OAuth 和凭据，不用散落在各配置文件里 |
| Gateway | 对 HTTP 类型的 Server 提供统一代理入口 |
| Skills | 引用 MCP Server 的可复用 AI 技能 |

## 三种 MCP 安装方式

| 方式 | 说明 |
| ---- | ---- |
| **npm** | `npx -y some-mcp-server`，适合 npm 发布的 Server |
| **Smithery** | 在网页上搜到，点安装，自动写配置 |
| **GitHub 直装** | `uvx --from git+https://...`，适合只有 GitHub 仓库的 |

三者不互斥。一般流程：先去 Smithery 搜有没有 → 没有再 GitHub 找。

## 对你的价值

当前工作流里，很多辅助工具需要手动配：

| 工具 | 现状 |
| ---- | ---- |
| Sequential Thinking | 已知 npm 包，直接装 |
| Memory MCP | 知道名字，还没研究 |
| Playwright MCP | 知道有，没配 |
| Context7 | 笔记本里有，没装 |
| DeepWiki | 笔记本里有，没装 |
| Grok-Search + Firecrawl | 笔记里有，没装 |

这些大概率全在 Smithery 上，搜一下就能装，不用一个一个去 GitHub 翻 README。

## 数据

- 6000+ MCP Server（截至 2026 年）
- 官方 API：`https://api.smithery.ai`
- SDK：`@smithery/cli`
- 开源仓库：https://github.com/smithery-ai

## 关键 URL

| 入口 | 链接 |
| ---- | ---- |
| 首页 | https://smithery.ai |
| 搜索 | https://smithery.ai/search |
| 文档 | https://smithery.ai/docs |
| API Keys | https://smithery.ai/account/api-keys |
| GitHub | https://github.com/smithery-ai |

## 来源

- 官网：https://smithery.ai
- GitHub：https://github.com/smithery-ai
- DeepWiki：https://deepwiki.com/smithery-ai/docs

## 相关

- [[Sequential Thinking]] — 结构化推理 MCP Server
- [[Trellis + GitNexus + Serena 工作流]] — 核心工作流
- [[Trellis 工序与工具链]] — 完整工具链推荐（大部分在 Smithery 上都能找到）
- [[工作流缺口分析]] — 辅助工具层缺口
