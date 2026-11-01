---
title: "OpenAI Agents SDK"
url: "https://github.com/openai/openai-agents-python"
source: "GitHub / openai"
published: "2025-03-11"
archived: "2026-10-02"
category: "Agent框架"
tags: [OpenAI Agents SDK, 多 Agent 协作, Handoff, Guardrails, Python]
lang: en
status: ok
---

# OpenAI Agents SDK

> 一句话定位：OpenAI 官方开源的轻量级多 Agent 工作流框架，用 Agent、Handoff、Guardrails、Sessions 四个原语把多角色协作跑起来，且不绑定自家模型。

## 摘要

OpenAI Agents SDK 是 OpenAI 官方开源的轻量级多 Agent 工作流框架（Python 包 `openai-agents`），核心抽象是 Agent、Handoff、Guardrails 与 Sessions：可以定义多个各自带指令、工具与安全防护栏的 Agent，让它们互相交接任务，或把一个 Agent 当作另一个 Agent 的工具来调用。框架内置人机协作（human-in-the-loop）、自动管理对话历史、全链路 Tracing 调试，并支持基于实时语音模型构建语音 Agent。虽由 OpenAI 维护，但它不绑定自家模型，官方称可接入 100 多个 LLM。仓库创建于 2025-03-11，MIT 许可，API 快照 29,799 stars、4,843 forks，主语言 Python，2026-10-02 仍有推送，文档站为 openai.github.io/openai-agents-python。

## 要点

- **四个核心原语**：Agent（指令 + 工具 + 防护栏）、Handoff（Agent 间任务交接）、Guardrails（输入输出安全检查）、Sessions（自动维护对话历史）。
- **不绑定模型**：`pip install openai-agents` 即可安装，可接 100 多个 LLM，非 OpenAI 模型同样可用。
- **Agent 可组合**：一个 Agent 能作为另一个 Agent 的工具被调用，适合把复杂流程拆成专职子角色。
- **工程配套**：内置 human-in-the-loop 人工介入、Tracing 全链路追踪、Realtime Voice Agent（源笔记称基于 gpt-realtime-1.5）。
- **关注度**：API 快照 29,799 stars / 4,843 forks，MIT 许可，主语言 Python，仓库持续活跃。

## 关联

- `Agent框架/OpenSRE.md` — 同属 Agent 编排框架，OpenSRE 可视为这套编排思路在 SRE 事故响应场景的垂直落地。
- `Agent框架/Mem0.md` — 框架自带的 Sessions 负责会话内历史，Mem0 补的是跨会话长期记忆层。

## 注意

- 源笔记标称「2.5 万+ stars」，GitHub API 快照为 29,799，属快照时间差异。
- Realtime Voice Agent 所用模型名（gpt-realtime-1.5）来自源笔记，未在 API 元数据中核验。
