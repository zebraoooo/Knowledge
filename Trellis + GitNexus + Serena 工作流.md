---
title: Trellis + GitNexus + Serena 工作流
created: 2026-06-05
updated: 2026-06-05
status: 草稿
type: 技术
tags: [trellis, gitnexus, serena, ai, dev-workflow, mcp]
aliases: [AI 编程工作流, 三件套]
---

# Trellis + GitNexus + Serena 工作流

三者分工明确，覆盖 AI 编程的**全流程**：从规范到理解到修改，各管一段。

## 一句话定位

| 工具 | 角色 | 解决什么问题 |
| ---- | ---- | ------------ |
| Trellis | 规范 + 工序编排 | AI 每次改代码风格不一样、会话失忆、规范散落各处 |
| GitNexus | 项目结构感知 | AI 看不懂大项目的代码结构，改一个地方不知道会影响什么 |
| Serena | 精准代码修改 | AI 改代码靠正则搜文本，而不是理解符号语义，乱改 |

**Trellis 管"做什么、怎么做"，GitNexus 管"看明白项目"，Serena 管"改得准"。**

## 为什么要组合

单独用任何一个都不够：

- **只用 Trellis**：工序很清晰，但 AI 读不懂大项目的代码关系，改起来还是瞎撞
- **只用 GitNexus**：项目看得懂，但没有规范约束，同一个需求三次生成三种写法
- **只用 Serena**：改得精准，但不知道为什么要改、改完有没有遗漏、下次会话全忘

三者合在一起才完整：

```
Trellis（规范驱动 + 工序编排）
    ↓ 告诉 AI 项目约定和当前任务
GitNexus（代码知识图谱）
    ↓ 帮 AI 看懂调用链、影响面、架构
Serena（语义级编辑）
    ↓ 让 AI 精准下手改代码
```

## Trellis：规范 + 编排

**核心观点：代码不再是资产，规范才是资产。**

四个核心概念：

- **Spec** → `.trellis/spec/`，项目编码规范，按模块分文件
- **Workspace** → `.trellis/workspace/`，会话日志，跨会话不丢上下文
- **Task** → `.trellis/tasks/`，工作单元，完整生命周期
- **Skill** → `.agents/skills/`，自动触发的工作流

五个自动触发技能：

- `trellis-brainstorm` — 一问一答澄清需求，起草 PRD
- `trellis-before-dev` — 改代码前读取受影响模块的 spec
- `trellis-check` — 实现完后对照 spec 审查 diff
- `trellis-update-spec` — 有价值的知识自动固化为规范
- `trellis-break-loop` — 修完 bug 后五维根因分析

典型流程：描述需求 → brainstorm → `/trellis:continue` → 实现 → `/trellis:continue` → 检查 → `/trellis:finish-work`

## GitNexus：看懂项目

零服务器代码知识图谱引擎，Tree-sitter AST + Graph RAG。

核心能力：

- **调用链追踪**：谁调谁、怎么调的，一路跟到底
- **影响面分析**：改这个函数会波及哪些地方
- **架构可视化**：项目模块怎么组织的，执行流程怎么走的
- **API 消费者映射**：每个接口谁在调，改了会断什么
- **跨仓库感知**：Monorepo 或多仓库之间的依赖关系

Trellis 官方推荐组合就是 Trellis + GitNexus，认为二者构成"完整 AI 工程方案"。

## Serena：精准修改

给 AI 编码助手的 IDE，通过 MCP 协议提供符号级语义操作。

两个后端：

- **Language Server（免费）**：LSP 协议，40+ 语言，能做符号查找、引用、声明、实现
- **JetBrains 插件（付费）**：直接调 IDE 分析引擎，额外能做文件移动、重命名重构、交互式调试

核心能力：

- **检索**：符号级搜索（不是正则搜文本）、文件大纲、查找引用、类型层级
- **重构**：作用域正确的重命名、移动符号/文件、安全删除
- **编辑**：替换符号体、符号前后插入代码
- **调试**：设断点、看变量、逐步执行（仅 JetBrains 后端）

## 跟 GitNexus 的关系

Serena 和 GitNexus 不是竞品，是上下游：

- **GitNexus 先上**：帮 AI 看懂项目全貌 —— 调用链、影响面、架构
- **Serena 再上**：帮 AI 下手改得准 —— 跳定义、查引用、安全重命名

一个负责理解，一个负责执行。

## 实际工作流

```
1. Trellis brainstorm → 明确需求，起草 PRD
2. Trellis before-dev → 读取相关 spec，AI 了解项目约定
3. GitNexus 探索 → 看懂受影响模块的调用链和影响面
4. Serena 修改 → 语义级精准编辑，不是正则瞎替换
5. Trellis check → 对照 spec 审查 diff，跑 lint/typecheck/test
6. Trellis finish-work → 归档，有价值的知识固化为 spec
```

每一步都有兜底：Trellis 管规范不跑偏，GitNexus 管理解不出错，Serena 管修改不漂移。

> [!warning] 已知缺口
> 核心链条完整，但上下游和侧翼存在五个缺口：测试、模型分工、辅助工具、CI/部署跟进、Debug 流程。详见 [[工作流缺口分析]]。

## 关键原则

- **规范驱动，不是感觉驱动**：先有 spec 再动手，消除 vibe coding
- **先看懂再改**：GitNexus 看清影响面，不改出连锁故障
- **语义级修改，不是文本替换**：Serena 确保重命名和重构不出低级错误
- **每次会话不丢上下文**：Trellis Workspace Journal 跨会话记住项目状态
- **人做最后兜底**：AI 产出后逐项核对，不盲目信任

## 安装与配置

按 Trellis → GitNexus → Serena 的顺序装，三个都配好才算完整。

### Trellis

**环境要求**：Node.js ≥ 18.17.0，Python ≥ 3.9

```bash
# 全局安装 CLI
npm install -g @mindfoldhq/trellis@latest

# 在项目根目录初始化（-u 指定你的名字，用于 Workspace Journal）
trellis init -u your-name

# 指定平台（默认同时启用 Cursor + Claude Code）
trellis init -u your-name --claude --codex
```

初始化后生成 `.trellis/` 目录：

| 目录/文件 | 作用 |
| --------- | ---- |
| `.trellis/spec/` | 项目编码规范，按模块分文件 |
| `.trellis/tasks/` | PRD、实现上下文、任务状态 |
| `.trellis/workspace/` | 个人会话日志 `journal.jsonl`，跨会话记忆 |
| `.trellis/.developer` | 你的用户名 |
| `.trellis/.version` | Trellis 版本（迁移用） |

三个核心命令：

```bash
/trellis:start        # 开启会话，读取规范、Git 状态、活跃任务
/trellis:continue     # 推进下一步，AI 自动判断当前阶段
/trellis:finish-work  # 收尾归档（前提是代码已 commit）
```

**注意**：macOS/Linux 如果 Python 命令找不到，设置 `export TRELLIS_PYTHON_CMD=/usr/bin/python3`。

### GitNexus

**环境要求**：Node.js ≥ 20

```bash
# 全局安装
npm install -g gitnexus

# 在项目根目录索引代码库（含 analyze + Skills 安装 + Hook 注册 + AGENTS.md 生成）
npx gitnexus analyze

# 自动检测已安装编辑器并写入 MCP 配置
npx gitnexus setup
```

或者手动注册 MCP：

```bash
# Claude Code（Windows 用 cmd /c）
claude mcp add gitnexus -- npx -y gitnexus@latest mcp
```

常用命令：

| 命令 | 用途 |
| ---- | ---- |
| `gitnexus analyze [path]` | 索引仓库（过期自动更新） |
| `gitnexus analyze --force` | 强制全量重建索引 |
| `gitnexus status` | 当前仓库索引状态 |
| `gitnexus clean` | 删除当前索引 |
| `gitnexus serve` | 启动 Web UI（HTTP 模式） |
| `gitnexus wiki [path]` | 从知识图谱生成 LLM 文档 |

**注意**：`.gitnexus/` 目录存储索引，应加入 `.gitignore`；代码变更后需重新 `analyze`；开启 `--embeddings` 可提升语义搜索质量但索引更慢。

### Serena

**环境要求**：Python 3.13（推荐 `uv` 管理）

方式一（推荐，uvx 直接启动）：

```bash
claude mcp add serena -- uvx --from git+https://github.com/oraios/serena serena start-mcp-server --context ide-assistant --project $(pwd)
```

方式二（全局安装）：

```bash
uv tool install -p 3.13 serena-agent
serena init
```

**关键配置坑位**：

1. **必须关闭 Web Dashboard**：在 `~/.serena/serena_config.yml` 设置：
   ```yaml
   web_dashboard: false
   web_dashboard_open_on_launch: false
   ```
   否则 Web Dashboard 的日志输出会干扰 MCP 的 JSON-RPC 通信。

2. **命令名要对**：`serena start-mcp-server`（不是 `serena-mcp-server`）

3. **用全局配置**：`~/.claude.json`（不是项目级 `.mcp.json`），避免重复注册

4. **Windows 坑**：PowerShell 下 `uvx --from` 传参可能异常，建议用 WSL2 或本地 clone + `uv run` 方式

验证连接：

```bash
# 健康检查
uvx --from git+https://github.com/oraios/serena serena project health-check

# Claude Code 里查看 MCP 状态
/mcp
```

### 环境自检清单

每次开工前扫一遍，确保三个都活着：

```
□ Trellis: .trellis/ 目录存在，trellis -v 正常
□ GitNexus: gitnexus status 返回索引状态
□ Serena: /mcp 里 serena 连接正常
□ 工具不是越多越好，用多少开多少
```

## 官方文档

| 工具 | 文档 |
| ---- | ---- |
| Trellis | [GitHub](https://github.com/mindfold-ai/Trellis) · [DeepWiki](https://deepwiki.com/mindfold-ai/Trellis/1.1-getting-started) · [npm](https://www.npmjs.com/package/@mindfoldhq/trellis) |
| GitNexus | [npm](https://www.npmjs.com/package/gitnexus) · [LobeHub MCP](https://lobehub.com/mcp/zasuozz-oss-ag-unity-gitnexus) · [CSDN 指南](https://blog.csdn.net/fogdragon/article/details/160776745) |
| Serena | [GitHub](https://github.com/oraios/serena) · [文档](https://oraios.github.io/serena/) · [Discord](https://discord.com/invite/cVUNQmnV4r) |

## 相关

- [[Trellis]] — 规范驱动开发框架
- [[Trellis 工序与工具链]] — 社区实战工序 + 完整工具链推荐
- [[Serena]] — IDE 级语义代码操作
- [[Sequential Thinking]] — 结构化推理引擎
- [[Smithery]] — MCP 应用商店
- [[Code Change Workflow]] — 单次代码修改 SOP
- [[工作流缺口分析]] — 当前工作流改进方向
