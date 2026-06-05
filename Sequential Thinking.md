---
title: Sequential Thinking
created: 2026-06-05
updated: 2026-06-05
source: https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking
status: 草稿
type: 技术
tags: [mcp, ai, reasoning, tools, claude-code]
aliases: [顺序思考, 结构化推理]
---

# Sequential Thinking

Anthropic MCP 官方团队的 MCP Server，给 AI 装一张草稿纸 —— 强制分步推理、可回退修改、可分支探索。社区评价：「金字招牌，任何时候都不会过时」。

不是解决某个具体问题，而是**提升 AI 推理质量本身**。

## 解决了什么问题

AI 面对复杂问题时容易犯两种错：要么凭直觉跳到结论（跳步），要么想错了就一路错到底（不回看）。Sequential Thinking 让 AI 像人类一样用草稿纸推演，推错了能改，推到一半发现量不够能加。

## 工作原理

一个名为 `sequential_thinking` 的 MCP 工具，AI 每次调用传入：

| 参数 | 说明 |
| ---- | ---- |
| `thought` | 当前这一步的思考内容 |
| `thoughtNumber` | 当前是第几步（≥1） |
| `totalThoughts` | 预估总共几步（可动态调整） |
| `nextThoughtNeeded` | 是否还需要下一步 |
| `isRevision` | 是否回头修正之前的某一步 |
| `revisesThought` | 在修正第几步 |
| `branchFromThought` | 从第几步分支出新路径 |
| `branchId` | 分支标识 |
| `needsMoreThoughts` | 是否需要扩充步数估计 |

完整的使用示例：

```
Step 1: problem decomposition — "这是一个登录模块的重构需求，涉及 Auth/Api/Middleware/UI 四层"
Step 2: survey context — "查看 Auth.ts 的现有实现，发现它耦合了 session 逻辑"
Step 3 (revision): revise Step 2 — "不对，session 并不是耦合在 Auth 里，是我看错了"
Step 4: explore impact — "改 Auth 会波及 Middleware 层，让我查调用链"
Step 5: plan approach — "先抽 session → 再改 Auth → 最后验证 Middleware"
Step 5 (branch A): "换一种思路：不改 Auth，直接从 Middleware 层加 interception"
Step 5 (branch B): "或者只改接口签名，内部保持现状"
Step 6 (compare branches): "branch B 风险最小，但同时值也小；branch A 最优比较"
...
```

## 安装与配置

在 Claude Code 中注册 MCP：

```bash
claude mcp add sequential-thinking -- npx -y @modelcontextprotocol/server-sequential-thinking
```

或手动写入 `~/.claude.json`：

```json
{
  "mcpServers": {
    "sequential-thinking": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
    }
  }
}
```

验证连接：

```text
/mcp
```

## 适用场景

| 场景 | 为什么需要它 |
| ---- | ------------ |
| 架构设计 | 涉及多层、多模块，一步想不全 |
| 需求拆分 | 从一句话需求到可执行子任务，需要多步推演 |
| Bug 排查 | 假设 → 验证 → 推翻 → 再假设，天然是分步分支的过程 |
| 影响面分析 | 改一个地方，要追踪 N 条调用链，分步跟不容易漏 |
| 代码审查 | 多个维度（安全、性能、可维护性）并行评估 |

## 不适用场景

- 简单问答（一个工具调用就能搞定的事不需要草稿纸）
- 纯执行类操作（写文件、跑命令）

## 跟其他工具的关系

工具体系里的分工：

| 工具 | 管什么 |
| ---- | ----- |
| Sequential Thinking | **管「想清楚」**—— 推理、规划、回溯 |
| GitNexus | **管「看懂项目」**—— 调用链、影响面、架构 |
| Serena | **管「改得准」**—— 符号级语义编辑 |

典型的配合方式：拿到复杂需求后，先开 Sequential Thinking 拆思路 → 再用 GitNexus 看代码结构 → 最后 Serena 下手改。

## 核心优势

- **官方出品**：Anthropic 自己写的，维护和兼容性有保障
- **零依赖**：不需要本地部署服务器，npx 一次拉起
- **MIT 协议**：随便用
- **通用**：不限语言、不限场景，纯粹做推理增强

## 安装方式

npm 包——`@modelcontextprotocol/server-sequential-thinking`。也有 Go、Python、Docker 版本的社区实现，功能一致。

## 来源

- GitHub: https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking
- npm: https://www.npmjs.com/package/@modelcontextprotocol/server-sequential-thinking

## 相关

- [[Trellis + GitNexus + Serena 工作流]] — 核心工作流
- [[Trellis 工序与工具链]] — 完整工具链推荐
- [[Smithery]] — MCP 应用商店
- [[工作流缺口分析]] — 辅助工具层缺口
