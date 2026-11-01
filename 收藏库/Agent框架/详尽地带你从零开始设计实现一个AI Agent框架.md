---
title: "详尽地带你从零开始设计实现一个AI Agent框架"
url: "https://mp.weixin.qq.com/s?__biz=MjM5ODYwMjI2MA==&mid=2649801169&idx=1&sn=0158c8b66288f1d0043f6e87363b9a4a"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent, Agent框架]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/详尽地带你从零开始设计实现一个AI Agent框架.html"
---

# 详尽地带你从零开始设计实现一个AI Agent框架

> 一句话定位：从 ReAct 理论到 279 行 Python 极简 Agent 框架的完整拆解

## 摘要

文章从理论到实践，带读者从零实现一个极简 AI Agent 框架。理论篇梳理 ReAct、Plan-and-Execute、Reflection 三大模式及 LangChain、LlamaIndex、AutoGen、CrewAI、LangGraph、Semantic Kernel 等主流框架选型，指出 Agent 工程核心是上下文工程，运行引擎是 Agent Loop。实践篇给出三层架构图，拆解 LLM Call、Tools Call、Context Engineering 三要素，用约 279 行 Python 单文件实现 while 循环驱动的 Agent Loop 与四个工具，配 DeepSeek、OpenAI SDK 与 CLI REPL 拼出可运行的极简 Agent，最后总结极简与上下文工程的意义。

## 要点

- Agent = Reasoning + Acting，核心实践以 ReAct 为基础，把推理与执行结合起来。
- 主流框架按场景选型：原型用 LangChain、RAG 用 LlamaIndex、多智能体用 AutoGen/CrewAI、复杂流程用 LangGraph、.NET 生态用 Semantic Kernel。
- Agent 工程上分 LLM Call、Tools Call、Context Engineering 三大部分，最大的变量是上下文工程。
- Agent Loop 本质是 while 循环：每次迭代做 LLM 推理、工具调用与上下文更新，无工具调用即结束。
- CodeAct 共识：用可执行代码统一行动空间，并把文件系统作为上下文。
- 实践用 DeepSeek deepseek-chat + OpenAI SDK 实现约 279 行单文件框架，提供四个工具并配 Python CLI REPL。
- 极简框架五脏俱全，能帮人看清 Agent 本质；代码库越简单上下文越清晰，Agent 越智能。

## 提炼

> 一句话定位：从 ReAct 理论到 279 行 Python 极简 Agent 框架的完整拆解

### 分节提炼

- **01 理论篇：三大模式与框架对比**：Agent 核心 = 推理 + 执行，ReAct 是基础模式：推理、执行、观察三步循环迭代。；Plan-and-Execute 先制定完整分步计划再执行，适合依赖明确的长期任务。；Reflection 用语言反馈而非权重更新强化 Agent，Self-Refine 平均提升约 20%。；工程上分 LLM Call、Tools Call、Context Engineering 三部分，核心是上下文工程。
- **02 实践篇：极简框架实现**：架构分 User Interface、Agent Loop Core、Tools Registry 三层，Loop 内 LLM Call→Tool Call→执行→回写上下文。；LLM Call 极简设计：DeepSeek deepseek-chat + OpenAI SDK，同步非流式调用。；Tools 用字典 name→(function, OpenAI schema) 注册，遵循 OpenAI Function Calling 格式。；System Prompt 极简告知可用工具与思考方式；messages 列表累积全部上下文。
- **03 结语：极简与上下文工程**：极简框架五脏俱全，帮人摒除复杂组件库、看清 Agent 本质。；代码库本质也是文件，将成为上下文工程一部分，代码越简单上下文越清晰。；上下文工程（长期/短期记忆、Session 管理、动态 RAG 等）是 Agent 智能与商业应用的关键。

### 带走三句话

1. Agent 框架本质 = 推理(LLM Call) + 执行(Tools Call)，核心在两者之间的上下文工程。
2. Agent Loop 就是一个 while 循环，把工具结果回写上下文，直到无工具调用即完成。
3. 极简实现能看清本质：四个工具 + 279 行代码即可跑通一个真实 Agent。

### 知识卡

跨全文挖掘 11 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
