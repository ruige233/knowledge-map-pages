---
title: "Graphify"
url: "https://github.com/Graphify-Labs/graphify"
source: "GitHub · Graphify Labs"
published: "2026-04-03"
archived: "2026-11-03"
category: "AI编程"
tags: [知识图谱, Claude Code, MCP, tree-sitter, 代码分析]
lang: en
status: ok
---

# Graphify

> 一句话定位：把整个代码库（含文档、SQL、配置、PDF）映射为可查询的知识图谱，以 `/graphify` skill 接入 20+ AI 编程助手，用图遍历替代 grep 式检索。

## 摘要

Graphify 是一个开源（Apache-2.0）代码知识图谱工具，把项目中的代码、文档、SQL schema、配置与 PDF 等映射为可查询的知识图谱，以 `/graphify` skill 形式接入 Claude Code、Cursor、Codex、Gemini CLI 等 20+ AI 编程助手。代码部分用 tree-sitter AST 本地确定性解析，不调用 LLM、不使用向量库；每条边带 `EXTRACTED` / `INFERRED` 置信标签，可区分“直接读到”与“推断得出”。运行后产出 graph.html（交互式图）、GRAPH_REPORT.md 与 graph.json 三个文件，支持 query / path / explain 三种查询。官方基准显示 LOCOMO recall@10 达 0.497、图构建 0 LLM credits；另有 Graphify Cloud 提供常驻在线版。

## 要点

- 安装接入：`uv tool install graphifyy`（PyPI 包名为 graphifyy，注意双 y），再 `graphify install` 自动检测并注册 `/graphify` skill；支持 macOS / Linux / Windows 与 20+ 助手，项目级可用 `--project`
- 本地确定性解析：代码经 tree-sitter AST 处理，无 LLM、不出本机；文档、PDF、图片、音视频等非代码内容才走模型语义解析（需配置 API key）
- 产物三件套：graph.html（浏览器点击/筛选/搜索）、GRAPH_REPORT.md（关键概念、意外关联、建议问题）、graph.json（完整图，含 `graph.schema_version` 与 `graph.graphify_version`）
- 查询方式：`graphify query "<问题>"` 返回子图、`graphify path A B` 追踪最短路径（整体置信度取最弱一跳）、`graphify explain X` 解释节点
- 跨 ~40 种语言解析 `calls` / `imports` / `inherits` / `mixes_in`，Leiden 社区检测与 God nodes；`# NOTE:` / `# WHY:` 注释和 ADR/RFC 引用成为一等节点
- 官方基准：LOCOMO recall@10 为 0.497（mem0 0.048、supermemory 0.149），LongMemEval-S QA 76%，图构建 LLM credits 为 0；快照数据：stars 约 141.6k、forks 约 13.5k（2026-11-02，滚动值），Python，默认分支 v8

## 关联

- 与 `Agent框架/给270万行代码开“上帝视角”：Agent 自主定位根因实践.md` 同属“代码知识底座 + Agent”思路：前者用于排障定位，Graphify 用于编码问答与代码理解
- 与 `Agent框架/【AI 创新实践】“架构师 Agent” 系统化落地.md` 互补：该文强调面向领域的强结构知识库，Graphify 提供自动化的代码知识图谱构建
