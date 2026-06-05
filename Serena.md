---
title: Serena
created: 2026-06-05
updated: 2026-06-05
source: https://github.com/oraios/serena
status: 草稿
type: 技术
tags: [mcp, ai, dev-tools, code-intelligence, lsp]
aliases: []
---

# Serena

The IDE for Your Coding Agent —— 给 AI 编码助手的 IDE。通过 MCP 协议给 Claude Code、Codex、Copilot 等工具提供**符号级别的语义理解和操作能力**。

## 核心价值

AI 写代码最大的问题不是智商不够，是**看不懂代码结构**。传统方式 AI 只能读文件、搜正则，而 Serena 让它像人类开发者一样用 IDE 的跳转定义、查找引用、重构等能力。

三句话概括：
- 给 AI 提供 IDE 级别的代码理解（不是 grep，是符号级语义分析）
- 让 AI 改代码更准（重命名不只是替换文本，是理解作用域的重构）
- 大项目尤其明显 —— 不用读完整个代码库才知道去哪改

## 技术架构

两个后端，二选一：

**Language Server（免费，默认）**
- 基于 LSP 协议，支持 40+ 语言
- Python、Java、Go、Rust、TypeScript、C/C++、Dart、Kotlin、Swift 等等
- 能做：找符号、文件大纲、查找引用、找声明、找实现、诊断

**JetBrains 插件（付费，有试用）**
- 直接调 JetBrains IDE 的分析引擎
- 比 LSP 更强：能做文件/目录移动、inline 重构、类型层级查询、交互式调试（设断点、看变量、逐步执行）
- 支持所有 JetBrains IDE 支持的语言

## 核心能力

### 检索
- 按符号名查找（不搜文本，搜语义）
- 文件大纲（一个文件里有哪些函数/类）
- 查找所有引用
- 类型层级（子类/父类关系）
- 查找声明和实现

### 重构
- **重命名**：不只是替换文本，作用域正确
- **移动**：移动符号、文件、目录（仅 JetBrains）
- **Inline**：把变量/函数内联展开（仅 JetBrains）
- **传播删除**：删符号时联动删掉未使用的引用（仅 JetBrains）

### 符号级编辑
- 替换符号体
- 在符号前/后插入代码
- 安全删除（不会删出语法错误）

### 交互式调试（仅 JetBrains）
- AI 能设断点、看变量、执行表达式、控制执行流
- 持久 REPL 风格

## 记忆管理

内置了一个简单的记忆系统，跨会话记住项目信息。可以和 AGENTS.md / CLAUDE.md 这类机制配合使用，也可以关掉。

## 安装

```bash
uv tool install -p 3.13 serena-agent
serena init
```

然后在客户端（Claude Code / Codex 等）的 MCP 配置里加上启动命令。

## 跟 GitNexus 的区别

Serena 是**实时改代码的**，背后是 LSP，提供跳转定义、重命名、引用查找这类 IDE 级操作。GitNexus 是**离线理解项目的**，背后是 Tree-sitter AST + 知识图谱，提供调用链追踪、影响面分析、架构可视化。

两者互补：GitNexus 让 AI 先看懂项目全貌，Serena 再帮它下手改得准确。

## 评价

linux.do 社区帖子：Codex 装了 Serena 之后准确度高了很多。

来自 README 中让 AI 自己评价 Serena 的结论：

> Opus 4.6："Serena 的 IDE 级语义工具是对我工具包最有力的补充——跨文件重命名、移动和引用查找，原本需要 8-12 个小心翼翼的步骤才能完成的操作，现在一次原子调用就搞定了。"

> GPT 5.4："作为编码 AI，我会让我的主人装 Serena，因为它给了我缺失的 IDE 级别的符号理解能力，把脆弱的文本手术变成了更冷静、更快、更自信的代码修改。"

## 相关

- [[Trellis + GitNexus + Serena 工作流]] — 三件套组合总结
- [[Trellis]] — 规范驱动开发框架
- [[Trellis 工序与工具链]] — 社区实战工序 + 完整工具链推荐

## 来源

- GitHub: https://github.com/oraios/serena
- 文档: https://oraios.github.io/serena/
- Discord: https://discord.com/invite/cVUNQmnV4r
