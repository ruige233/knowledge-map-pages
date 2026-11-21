---
title: "深入理解 AI Agent：设计原理与工程实践"
url: "https://github.com/bojieli/ai-agent-book"
source: "GitHub · bojieli"
published: "2025-09-09"
archived: "2026-11-21"
category: "教程"
tags: [Agent 研发, Agent, 上下文工程]
lang: zh
status: ok
---

# 深入理解 AI Agent：设计原理与工程实践

> 一句话定位：一本围绕「Agent = LLM + 上下文 + 工具」公式写成、正文与 109 个配套实验全部开源的中文 Agent 教材，从原理一路讲到生产级工程实践。

## 摘要

bojieli 著的开源书，用十章把 AI Agent 从原理推到工程落地：Agent 入门与 Harness 工程、上下文工程（KV Cache、提示工程、Agent Skills、上下文压缩）、用户记忆与知识库（RAG、结构化索引、知识图谱）、工具与 MCP、Coding Agent、交互与观察/动作空间扩展（异步事件、语音、Computer Use、机器人）、Agent 评估、模型后训练（SFT/RL）、Agent 持续进化、多 Agent 协作。全书正文、配图与 109 个配套实验全部开源，统一支持 Python 3.11–3.13 并用 uv 锁文件保证可复现，另提供 PDF/EPUB 与 15 种语言版本（中文 `book/` 为唯一正本）。2.0 版把原「异步交互」与「多模态 Agent」合并重组为新的第六章，其后章节依次后移。仓库 Apache-2.0，2025-09 创建，2026-11-21 约 5.9 万 stars。

## 要点

- **一条主线贯穿十章**：`Agent = LLM + 上下文 + 工具`；作者强调 Harness（循环、验证、观测等外围工程）才是竞争力所在，而非只换更强的模型。
- **章节与实验配比**（共 109 个）：上下文工程 10、记忆与知识库 12、工具/MCP 5、Coding Agent 16、交互扩展 14、评估 14、后训练 19、持续进化 9、多 Agent 6，第 1 章 4 个；实验分「可运行 / 复现 / 设计」三类。
- **实验环境**：章节维度装依赖 `uv sync --locked --extra ch1`（或 `pip install -e ".[ch1]"`），从仓库根跑 `uv run python chapter1/context/main.py`；带模型调用的实验需按各章 README 配 `.env`，仅 README 明确列出时才可切 `--provider ollama`。
- **外部依赖不进源码**：第 6、7、8、10 章的 22 个外部仓库 + 1 个训练 cookbook 需自行克隆，README 给了一键脚本并用不可变 SHA 做 `rev-parse` 相等性校验；部分章节实验要 Python 3.12+、浏览器/CUDA/FFmpeg 等系统依赖。
- **作者的阅读主张**：别只读正文，也别逐行读配套代码——配套代码本身就由 coding agent 依据书稿生成；应读懂架构与设计原则后让 AI 实现，人负责最初设计和最终验收。思考题参考答案在 `book/reference-answers.md`。
- **元数据快照**：主语言 Python，许可证 Apache-2.0，58,952 stars / 6,922 forks（2026-11-21 API 快照）；topics 覆盖 agent-memory、context-engineering、mcp、multi-agent、rag、reinforcement-learning 等。姊妹篇《深入理解 AI Infra：量化分析与系统设计》已单独开源。

## 关联

- `Agent框架/你不知道的 Agent：原理、架构与工程实践.md` — 同一组主题（Harness、上下文、工具、记忆、多 Agent、评测）的单篇文章版，可作为本书观点的浓缩前菜。
- `教程/Dive-Into-LLMs.md` — 同为公开免费的大模型教材，可对照「课程讲义型」与「专著 + 实验型」两种组织方式。

## 注意

- 摘要与要点基于仓库主页 README 与 GitHub API 元数据，未逐章抓取 `book/chapter*.md` 正文；章节细节以各章正文与各章实验 README 为准。
- stars / forks 是 2026-11-21 的 API 快照，README 的 Star History 图由 GitHub Action 每日自动重绘，属滚动值，会持续变化。
- `published` 取仓库创建日期（2025-09-09）；README 未给独立的成书/发布日期，书稿版本当前为 2.0（相对 1.4 有章节重组）。
- README 中「另含 1 个辅助训练 cookbook」等外部仓库版本固定于 2026-07-30 的 checkout / 上游审计，源码可克隆不代表实验已执行。
