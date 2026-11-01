---
title: "Mem0"
url: "https://github.com/mem0ai/mem0"
source: "GitHub / mem0ai"
published: "2023-06-20"
archived: "2026-10-02"
category: "Agent框架"
tags: [记忆层, Agent 记忆, 多信号检索, 实体链接, Python]
lang: en
status: ok
---

# Mem0

> 一句话定位：给 AI 助手和 Agent 用的可插拔长期记忆层，让应用记住用户偏好、跨会话延续上下文，并在多个记忆基准上明显超过旧版算法。

## 摘要

Mem0（读作 mem-zero）为 AI 助手和 Agent 提供一层可插拔的智能记忆基础设施，让应用记住用户偏好、跨会话延续上下文并随时间自我更新，适用于客服机器人、AI 助手与自主系统。检索侧采用多信号融合：语义相似度、BM25 关键词与实体匹配并行打分，并通过实体链接提升召回；官方强调是单次 LLM 调用的「单通道」设计，没有 Agent 循环，记忆累积而不覆盖。按 2026 年 4 月的新算法，LoCoMo 由 71.4 升至 91.6、LongMemEval 由 67.8 升至 93.4，单次约消耗 6.7–7.0K tokens、p50 延迟 0.88–1.09 秒；BEAM 在 1M/10M 规模上分别为 64.1 与 48.6。提供 Library、Self-Hosted、Cloud 三种部署形态，Apache-2.0 许可，API 快照 66,462 stars。

## 要点

- **多信号检索**：语义、BM25 关键词与实体匹配并行评分融合；实体跨记忆提取、嵌入并链接，用于提升检索效果。
- **单通道写入**：记忆累积不覆盖，一次 LLM 调用完成，不引入 Agent 循环，从而控制成本与延迟。
- **性能数据（2026-04 新算法）**：LoCoMo 91.6（旧 71.4）、LongMemEval 93.4（旧 67.8）、BEAM(1M) 64.1、BEAM(10M) 48.6；token 消耗 6.7–7.0K，p50 延迟 0.88–1.09s。
- **三种部署**：Library（`pip install mem0ai`，适合测试与原型）、Self-Hosted（`docker compose up`，团队自控基础设施）、Cloud（app.mem0.ai 全托管）。
- **接入方式**：Python SDK（`memory.add` / `memory.search`）、CLI（`mem0 init/add/search`），以及 ChatGPT 记忆、跨 ChatGPT/Perplexity/Claude 的浏览器扩展、LangGraph、CrewAI 等集成。
- **典型场景**：AI 助手长对话、客服历史工单复用、医疗健康偏好跟踪、生产力与游戏的自适应工作流。

## 关联

- `Agent框架/OpenAI-Agents-SDK.md` — 该框架的 Sessions 只管会话内历史，Mem0 提供跨会话长期记忆，两者可叠加使用。

## 注意

- 性能表与「2026 年 4 月新算法」的时间点来自源笔记，未在 GitHub API 元数据中核验。
- 源笔记未给发布日期，frontmatter 的 `published` 取仓库创建日期 2023-06-20。
