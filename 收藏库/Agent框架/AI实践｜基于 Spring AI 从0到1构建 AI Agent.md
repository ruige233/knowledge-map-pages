---
title: "AI实践｜基于 Spring AI 从0到1构建 AI Agent"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247559646&idx=1&sn=1abc5788cfe44b9820e5a0f4dfb3a336"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent框架, Spring, Agent]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/AI实践｜基于 Spring AI 从0到1构建 AI Agent.html"
---

# AI实践｜基于 Spring AI 从0到1构建 AI Agent

> 一句话定位：Spring AI 从0到1构建 AI Agent 六大模块实战拆解

## 摘要

本文以 AI 生成代码方式实现基于 Spring AI 的学习用 AI Agent Demo，拆解六大模块：AgentCore 编排意图识别、RAG 注入、记忆与模型调用；ChatMemory 三层压缩上下文；Tool 基于 Function Calling 可插拔注册；RAG 多路召回+RRF+Rerank；Command 与 Skill 两种 Prompt 模板；SubAgent 独立记忆；MCP 双向连接外部服务。结尾点明 Function Calling 是 Harness 大地基。

## 要点

- 项目为学习用 Demo，代码几乎全由 AI 生成，作者只做指挥与验收。
- AgentCore 编排完整对话流程：意图识别→RAG 注入→记忆管理→模型调用→工具执行（ReAct 循环）。
- ChatMemory 用三层策略控制上下文：摘要压缩、Assistant 消息裁剪、滑动窗口兜底。
- Function Calling 是 Harness 的大地基，Skill、SubAgent、RAG、MCP 能力多被包装为 Tool 供 LLM 调用。
- RAG 流水线：文档分块→向量化→多路召回（语义+BM25+查询改写）→RRF 融合→Rerank 重排。
- Command 与 Skill 同为 Markdown Prompt 模板：Command 用户主动调用，Skill 注册为工具由 LLM 决策。
- SubAgent 拥有独立 ChatMemory，与主 Agent 记忆隔离，生命周期由 LLM 通过工具调用驱动。
- MCP 双向支持：Server 对外暴露知识库检索，Client 动态连接外部服务并自动发现工具。

## 提炼

> 一句话定位：Spring AI 从0到1构建 AI Agent 六大模块实战拆解

### 分节提炼

- **01 AgentCore 核心编排器**：AgentCore 编排意图识别→RAG 注入→记忆管理→模型调用→工具执行的完整流程。；Agent Loop 由 Spring AI 的 ToolCallAdvisor 实现。；意图识别区分 RAG/GENERAL，避免每次对话都触发向量检索与 Rerank 开销。；ChatMemory 三层压缩：摘要压缩、Assistant 裁剪（保留 3 条）、滑动窗口兜底。
- **02 Tool 工具机制**：LLM 本身不调工具，真实执行在 Agent 服务端，LLM 只决定调用哪些工具。；工具实现 InnerTool 接口，启动时扫描 Bean 统一注册，新增无需改代码。；ChatClient 内置 ReAct 循环，LLM 可连续调用多个工具直至信息充足。
- **03 RAG 检索增强**：完整流水线：文档加载→分块→向量化→向量存储→多路召回→RRF 融合→Rerank→生成。；分块分确定规则与智能两类，默认 TextSplitter（500 字符/重叠 50）。；三路召回融合语义、BM25、查询改写，RRF 只看排名，适合融合异构结果。；VectorStore 为内存实现，生产环境可替换为 Milvus、Pinecone。
- **04 Command 与 Skill**：两者都是 Markdown 驱动的 Prompt 模板，但设计理念与使用方式不同。；Skill 带 Front Matter，扫描注册为 ToolCallback，LLM 自主调用。；Command 是纯 Prompt 模板，用户通过 REST API 指定命令名主动执行。；Command 是用户告诉 Agent 做什么，Skill 是 Agent 自己判断做什么。
- **05 SubAgent 子代理**：SubAgent 为需要独立上下文的长期任务设计，避免污染主对话记忆。；SubAgent 独立 ChatMemory，与主 Agent 隔离，共享 ChatClient。；生命周期（创建→多轮对话→销毁）由主 LLM 通过三个工具调用驱动。
- **06 MCP 双向连接**：MCP 是 Anthropic 提出的开放协议，项目同时实现 Server 与 Client。；MCP Server 暴露 knowledge_query，供外部 AI 应用复用。；MCP Client 优先 Streamable HTTP，失败回退 SSE，自动发现并持久化工具。
- **07 结尾感言**：LLM 对使用者本质只是你问我答，工程化聚焦于调整输入给 LLM 的内容。；从 Prompt 到 Context 再到 Harness，本质是决定上下文窗口放什么内容。；工具定义可能走到最后，Function Calling 是 Harness 的大地基。

### 带走三句话

1. 最好的学习资料是代码，AI Agent 能力看一遍实现就基本能通。
2. Function Calling 是 Harness 大地基，Skill/SubAgent/RAG/MCP 多被做成 Tool。
3. 工程化的本质是决定有限的上下文窗口里放什么内容。

### 知识卡

跨全文挖掘 11 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
