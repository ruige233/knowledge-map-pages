---
title: "MindsDB"
url: "https://github.com/mindsdb/mindsdb"
source: "GitHub / mindsdb"
published: "2018-08-02"
archived: "2026-10-02"
category: "AI应用"
tags: [SQL, 语义搜索, MCP, 数据平台, AI代理]
lang: en
status: ok
---

# MindsDB

> 一句话定位：用统一 SQL 方言把分散的数据库、数据仓库与 SaaS 应用连成一片的 AI 数据平台，让代理和人都能用自然语言或 SQL 跨源取数。

## 摘要

MindsDB 用统一 SQL 方言连接分散的数据系统，提供两块产品：ANTON 是个人 AI 代理，用自然语言下达需求后自动发邮件、调用 API、连数据源、建仪表板；Query Engine 是语义搜索查询引擎，可从 200+ 数据源检索分析数据。核心流程为 Connect → Unify → Respond：连接 Postgres、MongoDB、Slack、文件等数据源，由动态上下文引擎在知识库中融合结构化表格与向量化数据（文本、PDF、HTML），再由代理跨数据栈推理作答。支持跨源 JOIN、用 CREATE KNOWLEDGE_BASE 结合语义搜索与元数据过滤、用 CREATE AGENT 打包代理并经 API 或 MCP 集成。截至 2026-10-02 约 39.8k stars、6.25k forks，MIT 许可。

## 要点

- 双产品定位清晰：ANTON 面向个人自动化任务，Query Engine 面向用 SQL 跨 200+ 数据源做语义检索与分析。
- 核心抽象是 SQL：跨源 JOIN、`CREATE KNOWLEDGE_BASE`（语义 + 元数据过滤）、`CREATE AGENT`（指定模型、知识库与表集合）都用声明式语句完成。
- Connect → Unify → Respond 三步方法论把"接入数据源 / 融合结构化与向量数据 / 代理推理作答"拆开，便于定位问题环节。
- 集成途径含 API 与 MCP，可作为工具接入 AI 客户端；工作流用 jobs/triggers 管理。
- 关注度：约 39.8k stars、6.25k forks；MIT 许可；创建于 2018-08-02，最后推送 2026-09-16；topics 含 agents、mcp、anton、claude-cowork、codex 等。

## 关联

- 与 `AI应用/NoteGen.md` 相关：都涉及知识库、向量检索与 MCP 接入，但 MindsDB 面向企业数据栈而非个人笔记。
- 与 `开发工具/DeepGEMM.md` 同属 AI 基础设施向工具（推理/数据侧）。

## 注意

- API 返回主语言为 Makefile，源笔记未提及，故未写入摘要；仓库无正式发布日期，`published` 取创建日期 2018-08-02。
- 源笔记中的 SQL 示例语句与 Docker 启动参数未实测；"200+ 数据源"与"更低 token 消耗"等说法来自项目自述。
- 星标/forks 为 2026-10-02 快照，会随时间变化。
