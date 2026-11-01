---
title: "Learn Claude Code"
url: "https://github.com/shareAI-lab/learn-claude-code"
source: "GitHub / shareAI-lab"
published: "2025-06-29"
archived: "2026-10-02"
category: "教程"
tags: [Claude Code, Agent Harness, Agent Loop, 上下文压缩, Python]
lang: zh
status: ok
---

# Learn Claude Code

> 一句话定位：通过逆向拆解 Claude Code 的 12 个机制，教「模型之外的工程基础设施（Harness）」该怎么造。

## 摘要

shareAI-lab 的递进式教程，核心命题是「Agency 来自模型训练，Agent 产品 = 模型 + Harness」。课程用 s01–s12 十二课，每课只给固定不变的 agent loop 叠加一个 Harness 机制：Agent Loop、Tool Use、TodoWrite、Subagent、Skills、Context Compact、Task System、Background Tasks、Agent Teams、Team Protocols、Autonomous Agents、Worktree 任务隔离，工具数从 1 个累积到 16 个。教学上先讲问题、方案与 ASCII 图，再给最小化 Python 实现，`s_full.py` 把全部机制合一，并配英文、中文、日文三语文档与基于 Next.js 的交互式学习平台。作者把 Harness 工程师的职责归纳为实现工具、策划知识、管理上下文、控制权限、收集任务过程数据五点。MIT 许可，主语言 Python。

## 要点

- **12 课递进结构**：循环属于 Agent、机制属于 Harness——每课只加一个机制，agent loop 本身始终不变，这是理解课程划分的关键。
- **工具数量是可量化的难度标尺**：s01 只有 1 个工具（一个循环 + Bash 就够），s04–s06 为 5 个，s12 累积到 16 个。
- **上下文与并发是主线**：Subagent 提供干净的子上下文、Context Compact 压缩已满上下文、Task System 把拆解后的任务落盘、Background Tasks 让慢操作不阻塞推理、Worktree 让并行任务各用独立目录。
- **团队阶段（s09–s12）**：Agent Teams 分工、Team Protocols 统一沟通规矩、Autonomous Agents 从看板认领任务、Worktree 隔离落地，是单体 Agent 走向多 Agent 协作的四个坎。
- **可复用结论**：Agent 产品的差异化来自 Harness（工具、知识、上下文管理、权限边界），作者认为这套设计模式可迁移到编程之外的行业；配套还有 Kode Agent CLI 与 SDK。
- **元数据**：MIT 许可，主语言 Python；API 快照（2026-10-02）约 77,907 stars、12,501 forks，仓库创建于 2025-06-29。

## 关联

- `教程/Claude-Code-Best-Practice.md` — 实践侧的最佳实践，与本教程的原理侧拆解互补。
- `教程/Easy-Vibe.md` — 阶段三以实操方式覆盖 Claude Code、MCP、Skills 与 Agent Teams。

## 注意

- `published` 取仓库创建日期（2025-06-29），仓库没有正式发布日期。
- 本地笔记中的「Web 交互式学习平台」地址写的是 `http://localhost:3000`，那是本地开发端口而非公开访问地址，故未在摘要中引用。
- stars/forks 为 2026-10-02 抓取快照。
