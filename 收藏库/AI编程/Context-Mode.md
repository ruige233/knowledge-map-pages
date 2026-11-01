---
title: "Context Mode"
url: "https://github.com/mksglu/context-mode"
source: "GitHub / mksglu"
published: "2026-02-23"
archived: "2026-10-02"
category: "AI编程"
tags: [上下文压缩, MCP, SQLite, FTS5, Claude Code]
lang: en
status: ok
---

# Context Mode

> 一句话定位：给编码智能体做上下文窗口优化——先把工具输出压缩再进上下文，同时把会话状态持久化下来，让长任务不会因压缩而失忆。

## 摘要

项目解决 AI 编码工具上下文窗口快速耗尽的问题。做法是把工具输出先经沙箱化处理再交给模型，官方描述压缩率约 98%；源笔记给出的量级示例是 Playwright 快照从 56 KB 压到 299 字节、GitHub Issue 页面从 59 KB 压到 1.1 KB。它同时用 SQLite + FTS5 + BM25 建立可检索的持久记录，追踪文件编辑、Git 操作、任务状态和用户决策，因此即使对话被压缩，模型也能从上次中断处继续，源笔记称可把一次 AI 编码会话从约 30 分钟拉长到 3 小时。接入方式为 MCP 加 hooks，仓库 topics 列出 17 个平台，包含 Claude Code、Codex CLI、Copilot、Cursor、OpenCode、Zed、Kiro、OpenClaw 等，主语言 TypeScript。

## 要点

- **压缩路径**：不是简单截断，而是把工具输出沙箱化后摘要再注入上下文，压缩率约 98%。
- **量级示例**：Playwright 快照 56 KB → 299 字节；GitHub Issue 页面 59 KB → 1.1 KB。
- **记忆与检索**：SQLite + FTS5 + BM25 记录文件编辑、Git 操作、任务状态与用户决策，支撑压缩后按断点续做。
- **覆盖范围**：通过 MCP + hooks 接入 17 个平台（Claude Code、Codex CLI、Copilot、Cursor、OpenCode、Zed、Kiro、OpenClaw 等）。
- **关注度**：2026-10-02 API 快照 24,898 stars、1,790 forks；主语言 TypeScript，创建于 2026-02-23，最后推送 2026-10-02。

## 关联

- `AI编程/Claude-Context.md` — 同批条目，都围绕"省上下文"：本条压缩工具输出并保住会话记忆，那条从代码库检索相关内容。
- `Agent框架/Mem0.md` — 同为给智能体加持久记忆的能力，本条偏会话/任务状态，Mem0 偏长期记忆层。
- `AI编程/Everything-OpenCode.md` — 同批条目，都是给 OpenCode、Claude Code 等编码 CLI 增补能力的插件式方案。

## 注意

- 源笔记标注 Star 数"接近 1 万"，API 快照为 24,898，两者为不同时点数据，本条目按 API 记录。
- "会话从 30 分钟延长到 3 小时"及 "98% 压缩率"来自源笔记与项目自述，未独立验证。
- API 的许可证字段为 `NOASSERTION`，即未识别出标准许可证，故本条目不记录具体许可证。
