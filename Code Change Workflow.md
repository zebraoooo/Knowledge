---
title: Code Change Workflow
created: 2026-06-05
updated: 2026-06-05
source:
  - https://me.lettergen.io/ai-workflows/code-change-workflow-presentation.html
  - https://github.com/viviannnl/ai-workflows/blob/main/code-change-workflow.md
status: 草稿
type: 技术
tags: [ai, skill, workflow, tdd, code-review, ci]
aliases: [代码修改工作流, Hermes Agent]
---

# Code Change Workflow

Hermes Agent 出品的一套 AI 代码修改工作流 Skill。本质是一套**给 AI 编程助手的 SOP**：改代码之前先定义行为、写测试、最小实现、分层验证、独立审查、最后才提交推送。

一句话：**别上来就写代码。先搞清楚要改什么、怎么验证，再动手。**

核心原则：Do not jump straight into code. First define the exact intended behavior and success criteria; then prove the change with tests and/or manual verification; then review; then commit and push only after validation.

## 何时使用

适用于任何会改动代码仓库文件的任务：修 bug、新功能、UI 改动、重构、测试、构建/部署/配置、依赖更新。

不适用：只读调查、纯解释问题、代码库之外的一次性数据分析。

## 11 步工作流

1. **理解需求** — 重述 bug 或功能，看相关文件和当前行为，`git status` 确认分支和仓库状态
2. **定义成功标准** — 写代码前先写：预期行为、边界情况、测试列表、人工检查清单、不做什么
3. **创建任务计划** — 复杂任务拆成子步（审查 → 测试 → 实现 → 验证 → 审查 → 提交 → 推送 → CI）
4. **TDD（尽量）** — 红 → 绿 → 重构；先写失败测试，再做最小修复，让测试通过
5. **最小化实现** — 只改必要的，不碰无关文件和无关重构，不偷偷改产品行为
6. **分层验证** — 先跑针对性测试 → 再跑完整套件 → lint → typecheck → build
7. **本地人工测试** — UI 或应用流程必须打开真实页面测，检查组件的实际渲染效果和浏览器控制台
8. **独立审查** — `git diff` 检查范围、秘密信息、调试代码；重要改动用独立 sub-agent 再查一轮
9. **干净提交** — 只 stage 目标文件，用 conventional commit（`fix:` / `feat:` / `refactor:`）
10. **推送并创建 PR** — 验证后再推；PR 里写总结、测试计划、人工验证、截图、限制
11. **跟进 CI 和部署** — 推送后看 CI 日志，有失败就修好再推，检查部署域名

## TDD：红 → 绿 → 重构

修 bug 和改行为时最安全的方法：

1. **Red｜红**：先写失败测试，证明 bug 确实存在
2. **Green｜绿**：做最小安全修改，让测试通过
3. **Refactor｜重构**：测试通过后再清理代码

如果 TDD 不适用（纯样式微调、生成的代码、只能手动验证的外部配置），要说明为什么不用。

## 验证清单（15 项）

必须全部完成或标注阻塞才算"做完"：
- 已理解需求
- 已检查 Git 分支和状态
- 已定义成功标准
- 已写失败测试或解释为何不适用
- 已完成最小实现
- 针对性测试通过
- 相关完整测试通过或记录失败原因
- Lint/typecheck/build 通过或记录限制
- UI 相关已人工测试
- 已检查浏览器控制台
- 已审查 diff 范围和秘密信息
- 重要改动已独立审查
- 目标文件已提交
- 已推送/创建 PR
- 已检查 CI 和部署

## 常见坑

- 过早写代码（没定义目标行为就开始，通常会返工）
- 没有回归测试（修完的 bug 未来可能悄悄坏掉）
- 只测顺利路径（边界和失败状态同样重要）
- 提交无关文件
- 把 localhost 当线上（用户问"能不能看到"指的是部署，不是本地 dev server）
- 不跟进 CI（推送后不管了）
- UI 改动不看控制台
- 过度重构

## 一些有价值的细分场景

文档里还覆盖了这些具体场景的处理方式：

- **配置/模型版本变更**：改 LLM model name 之类，要加回归检查扫描旧标识符，确保参数兼容性
- **前后端数据库迁移**：生产数据库变更要人工审批，前端要覆盖 loading/success/empty/error 四种状态
- **LLM 分类/聚类/提取行为变更**：定义原子粒度，测试多语言输入，注意 LLM 路径和确定性 fallback 路径都要守卫
- **私密数据导出→审查流程**：分两阶段（本地工具化 → 后续导入），默认全部标 `approved: false`，过滤掉 token/邮箱/电话等敏感信息
- **公网 UI/内容完整性**：搜 prototype/demo 字符串、虚假营销声明、硬编码身份，不要从后端架构推断公开定位（内部 workspace ≠ 可以叫自己 B2B）
- **文档级产品决策**：用户最新措辞是 source of truth，不改无关段落，改完搜索旧措辞有没有残留

## 最终汇报格式

做完后向用户报告：
- 问题/功能描述
- 改了什么文件
- 实现了什么
- 测试和结果
- build/lint/typecheck 结果
- 人工测试记录
- 审查发现
- commit/PR/push 状态
- 剩余限制或风险

"Done" means verified, reviewed, and safe to ship.

## 来源

- 演示页：https://me.lettergen.io/ai-workflows/code-change-workflow-presentation.html
- GitHub：https://github.com/viviannnl/ai-workflows/blob/main/code-change-workflow.md
