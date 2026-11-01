---
title: "Claude Context"
url: "https://github.com/zilliztech/claude-context"
source: "GitHub / zilliztech"
published: "2025-06-06"
archived: "2026-10-02"
category: "AI编程"
tags: [MCP, 语义搜索, 混合检索, BM25, Claude Code]
lang: en
status: ok
---

# Claude Context

> 一句话定位：把整个代码库变成编码智能体可检索的上下文，用语义搜索替代"把所有文件塞进窗口"。

## 摘要

针对 AI 辅助编码时代码库过大、模型找不到相关代码的痛点，该项目把代码库做成可被任意编码智能体检索的上下文层。检索采用 BM25 关键词召回与稠密向量检索的混合方案，支持用自然语言直接定位代码位置；索引采用增量策略，只对发生变更的文件重新建索引，不需要全量扫描。源笔记称实测可节省约 40% 的 Token 消耗，因为不必把整个目录塞进上下文。它以 MCP 插件形式接入，客户端覆盖 Claude Code、Cursor、VS Code、Windsurf、Cline、Gemini CLI。仓库隶属于 zilliztech 组织，MIT 许可，主语言 TypeScript，GitHub topics 包含 `mcp`、`merkle-tree`、`embedding`、`vector-database`、`semantic-search`、`agentic-rag`。

## 要点

- **混合检索**：BM25 与稠密向量结合，兼顾关键字精确匹配与自然语言语义匹配。
- **增量索引**：只重建变更文件的索引，避免每次会话都全量扫描代码库；topics 中的 `merkle-tree` 暗示其变更检测与持久化索引结构。
- **Token 成本**：源笔记给出约 40% 的消耗节省，前提是不再把整个目录放进上下文。
- **集成方式**：作为 MCP 服务器接入，支持 Claude Code、Cursor、VS Code、Windsurf、Cline、Gemini CLI 等编码客户端。
- **关注度**：2026-10-02 API 快照 12,585 stars、938 forks；MIT 许可，主语言 TypeScript，创建于 2025-06-06，最后推送 2026-07-14。

## 关联

- `AI编程/Context-Mode.md` — 同批条目，同样解决"上下文不够用"，但那条压缩工具输出与持久化会话状态，本条负责从代码库中检索出该看的部分。
- `AI编程/DeepWiki-Open.md` — 同批条目，同样基于索引 + 检索让模型理解陌生代码库，本条面向编码时的实时检索，那条面向生成 Wiki 文档与问答。

## 注意

- 源笔记标注 Star 数"9000+"，API 快照已为 12,585，两者为不同时点数据，本条目按 API 记录。
- "约 40% Token 节省"与多平台支持列表来自源笔记（公众号推荐转述），本条目未在仓库 README 或官方基准中核验。
