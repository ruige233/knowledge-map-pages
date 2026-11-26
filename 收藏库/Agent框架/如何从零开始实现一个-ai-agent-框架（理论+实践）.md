---
title: "如何从零开始实现一个 AI Agent 框架（理论+实践）"
url: "https://mp.weixin.qq.com/s/z1aDaPFhjYb2cv-9bFbdaQ"
source: "微信公众平台 · 腾讯云开发者"
published: "2026-02-26"
archived: "2026-11-26"
category: "Agent框架"
tags: [Agent 研发, Agent, 上下文工程]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/如何从零开始实现一个-ai-agent-框架（理论+实践）.html"
---

# 如何从零开始实现一个 AI Agent 框架（理论+实践）

> 一句话定位：279 行单文件讲透 Agent Loop 与上下文工程，从理论到跑通。

## 摘要

一篇「理论 + 实践」双篇的从零手搓教程，作者是腾讯云开发者。开篇给了一句值得记的话：「人类最重要的 affordance 是『手』，而 AI 最重要的 affordance 可能是『代码』。」——这句话是全篇的暗线，最终指向 CodeAct 与「代码即行动空间」。

**理论篇**梳理三条源流。**ReAct**（Yao 等，2022）把 Reasoning 与 Acting 结合，补上 CoT 缺外部反馈的缺口，形成「推理 → 行动 → 观察」的三步循环，至今仍是所有 Agent 范式的底座。**Plan-and-Execute**（LangChain，2023，基于 Plan-and-Solve 论文与 BabyAGI）改为先出完整计划再逐步执行，适合关系明确的长期任务，代价是倾向 workflow、缺少动态调整。**Reflection** 一系不改模型权重、只改「下一次尝试」：Reflexion 把口头反思写进情节记忆，Self-Refine 在同一任务内迭代改进（评测任务平均 +20%），CRITIC 引入搜索与代码执行等工具来验证再修正。

随后是框架版图：LangChain（快速搭复杂应用）、LlamaIndex（RAG 与检索）、AutoGPT/AutoGen（多 Agent 协作）、CrewAI（角色扮演协作）、LangGraph（状态图流程控制）、Semantic Kernel（.NET 与 Azure 生态），并给出按场景选型的五条建议；同时点出新趋势——基于通用 Code Agent SDK 的「套壳 Agent」（如 CodeBuddy Agent SDK → WorkBuddy）。

**框架核心**由 Manus 的故事引出：Peak 那句「Actually, Manus doesn't use MCP」的后半句是「inspired by CodeAct」；Manus 放弃微调路线转攻上下文工程，主张用文件系统当上下文，三个月后 Claude Skills 让这一理念深入人心。由此得出两大业内共识——**用文件系统作为上下文**、**编程是解决通用问题的普适方法**；工程上则落成三大部分：LLM Call（标品，LiteLLM 已很成熟）、Tools Call（Function Call → MCP → Skill）、Context Engineering（唯一的大变量，也是 Agent 智能的核心）。作者引混元《从 Context 学习，远比我们想象的要难》：不给任何 Context 时，GPT-5.1 (High) 也只能解决不到 1% 的任务。而这一切的引擎是 **Agent Loop**——本质就是一个 While 循环，每轮 = LLM 推理 + 工具调用 + 上下文更新，某轮不再调工具即为完成。

**实践篇**用 DeepSeek（deepseek-chat）+ OpenAI SDK 同步非流式调用、四个工具（shell_exec / file_read / file_write / python_exec）、手动字典注册 name → (function, OpenAI Function Calling schema)、System Prompt + messages 列表作 session，完整给出 agent_loop()、工具实现、TOOLS 注册、SYSTEM_PROMPT、CLI REPL main() 全部代码——**单文件 279 行**。跑通效果：问候、查当前目录文件、统计代码行数与 token 数（过程中自主写代码并执行）、exit 退出。

**收尾**很坦诚：健壮性、安全性、流式输出、Tools 注册的优雅性都有改进空间，但它五脏俱全。作者给出极简的两条理由：一是便于讲清关键点，二是**代码库本身也是上下文工程的一部分**——代码越简单、上下文噪声越少，Agent 越聪明。

## 要点

- 全篇总纲：**Agent = Reasoning + Acting**。所有后续框架都是 ReAct 之后的扩展与补充，核心实践仍是「推理 + 执行」。
- ReAct 三步循环的分工：Reasoning 依赖 LLM 做 CoT 决策，Acting 执行具体操作（Function Tool / MCP / Shell / 代码执行，取决于宿主机环境），Observation 把结果回灌下一轮或判定可以收尾。
- ReAct 补的是 CoT 的缺口——CoT 让模型想得更好，但想不出外部世界的新信息；「行动 → 观察」把外部反馈接回循环。
- Plan-and-Execute 的结构是 `Planning → Task1 → Task2 → Task3 → Summary`；适合复杂且任务关系明确的长期任务，缺点是倾向 workflow、缺乏动态调整。
- Reflection 三条线：Reflexion（语言反馈替代权重更新）、Self-Refine（先输出再反馈迭代，平均 +20%）、CRITIC（清华 + 微软，用搜索与代码执行器验证输出再自我修正）。
- 六框架选型口诀：原型要快选 LangChain、做 RAG 选 LlamaIndex、多 Agent 选 AutoGen/CrewAI、流程控制选 LangGraph、.NET 生态选 Semantic Kernel。
- 新趋势：Claude Cowork 带火了「基于通用 Code Agent SDK 的套壳 Agent」，创新点在于**为具体用户场景做交互设计与工作流**，而不是造框架。
- Manus 的两条工程遗产：放弃微调、转攻上下文工程（2025.07 工程博客）；提出「使用文件系统作为上下文」（一个月后被 Claude Skills 发扬）。
- CodeAct（UIUC，2024）的意义：用生成可执行 Python 代码来**统一行动空间**——Acting 不只有 Function Call 和 MCP，还可以直接写代码，而且效果更好。
- Anthropic《Code execution with MCP》把 MCP 服务器当代码 API 暴露，Agent 写代码与之交互，从而**按需加载、更省上下文**。
- 三大部分的工程判断：LLM Call 几乎没有工程变量（LiteLLM 兜底）；Tools 有业内最佳实践、按场景增删；**Context Engineering 是最大变量，是 Agent 智能的核心**。
- 硬数据：不提供任何 Context 时，GPT-5.1 (High) 仅能解决不到 1% 的任务——「上下文工程是现在唯一低垂的果实」。
- Agent Loop 的终止条件只有一条：**某一次 Turn 不再执行工具**（assistant 消息里没有 tool_calls）就返回文本。完成与否由模型行为决定，而不是外部启发式猜。
- 三要素的极简实现：LLM 用 deepseek-chat + OpenAI SDK（同步非流式为可读性）；工具四件套覆盖「读、写、跑、算」；上下文用 System Prompt 定行为、messages 列表管状态。
- messages 的更新规则：`system` 初始化 → `user` 追增 → `tool` 回填结果；assistant 消息用 `model_dump()` 原样追加，保证 `tool_call_id` 对得上。
- 工具注册用手动字典 `name → {function, schema}`：key 是模型能吐出的名字，schema 是 OpenAI Function Calling 格式，二者成对出现才让分发可靠。
- 工具实现的统一约定：**永远返回字符串**（成功内容或 `[error] ...`），异常与 30s 超时都转成文本——错误因此变成模型可读的上下文，Agent 能自己纠偏而不是程序崩掉。
- 安全上限 `MAX_TURNS = 20` 是必须的刹车；原文一处写「20 轮（MAX_TURNS=200）」与代码不一致，以代码为准。
- 实测效果：能查目录、能自主写脚本统计代码行数与 token 数并执行、能给结论；还能用 `clear` 重置上下文、`exit` 退出。
- 作者的安全提醒：文件写 + Shell + 代码执行三权合一时，Agent 在本机「真的可以为所欲为」，教学可跑、生产必须补沙箱与权限。
- 值得一提的对照：OpenClaw 底层 Pi Agent 的 Tools 层也只有四个（Read / Write / Edit / Shell），丰富能力靠事件机制与 Skills 扩展——**极简不等于简陋**。
- 极简的两条理由：便于论述关键点；以及代码库本身也是上下文工程的一部分，代码越简单、上下文越清晰、Agent 越聪明。

## 关联

- 《一文拆解 DSH Super-agent 双层架构》是同一套三要素的工业化版本：把 Session 管理与上下文同步从 Agent Loop 里提到系统层（Router / Worker 双层）。
- 《换模型效果归零？一文扒透 Harness 落地 11 个坑》从反面呼应本文第十条要点：Harness 就是本文所说的 Context Engineering，是唯一的大变量，也是最多人做砸的地方。
- 《维护3.3万字符的长 Prompt？让优化 Agent 迭代执行 Agent》是本文 Agent Loop 的直接应用：一个 Agent 循环执行、一个 Agent 优化提示。
- 《AI 写代码越来越快，谁来守住章法？》补上本文点到即止的一环：给 Agent 的「代码即行动」加硬约束（Architect.md + ARCH-CHECK）。
- 《多Agent协作的下一场竞争，不在模型，而在上下文》把本文的上下文工程视角从单 Agent 扩展到多 Agent：协作本质上是在传递上下文。

## 提炼

- **ReAct vs Plan-and-Execute（概念对比）**：ReAct 边想边做、每步看观察反馈，灵活能纠偏；Plan-and-Execute 先出完整计划再逐条执行，稳定但容易固化成 workflow、缺动态调整。
- **Reflexion vs Self-Refine vs CRITIC（概念对比）**：Reflexion 把反思写进情景记忆影响后续试验；Self-Refine 在同一任务内迭代改进（平均 +20%）；CRITIC 引入搜索、代码执行等工具验证输出再修正。
- **MCP vs CodeAct（概念对比）**：MCP 直接暴露工具调用；CodeAct 让 LLM 生成可执行代码来统一行动空间。Manus 选了后者，效果更好，也带动了「MCP 服务器当代码 API」的新玩法。
- **三大部分的分工（结论）**：推理本质是 LLM Call，执行本质是 Tools Call（代码只是工具的一种），连接二者的 Context Engineering 才是框架核心与智能来源。
- **Agent Loop 就是一个 While（结论）**：每轮 = 一次 LLM 推理 + 工具调用 + 上下文更新；某轮不再调工具即完成；框架设计 = 在这个循环里设计如何管理上下文。
- **Manus 的两大共识（结论）**：使用文件系统作为上下文；编程是解决通用问题的普适方法（问题 → 生成代码 → 执行 → 再迭代，直到解决）。
- **代码库也是上下文工程（结论）**：代码越简单、上下文噪声越少，Agent 越聪明——这既是极简的理由，也是「把框架做厚」的反面警示。
- **无 Context 的 1%（数据）**：不提供任何 Context 时，最强模型 GPT-5.1 (High) 也解决不到 1% 的任务——上下文工程是当前唯一低垂的果实。
- **Self-Refine 提 20%（数据）**：先输出再自我反馈迭代，在全部评估任务上平均提升约 20%，不改权重只改迭代方式。
- **279 行 / 4 工具 / 20 轮（数据）**：整份框架单文件 279 行；工具 shell_exec、file_read、file_write、python_exec；循环上限 MAX_TURNS = 20；工具执行统一 30s 超时。
- **Agent Loop 骨架（代码模式）**：`for turn in range(1, MAX_TURNS+1)`，无 `tool_calls` 就 `return assistant_msg.content`，否则逐个执行工具并 append 结果，最后返回「reached maximum turns」。
- **TOOLS 字典注册（代码模式）**：`TOOLS[name] = {"function": fn, "schema": {...}}`，schema 走 OpenAI Function Calling 格式；LLM 说名字，字典找到函数执行——这一跳完全可见。
- **工具错误即文本（代码模式）**：每个工具 try/except 包住，超时或异常一律 `return "[error] ..."`，让错误回到上下文，而不是抛异常炸掉整个循环。
- **messages 漏交 tool 结果（易错点）**：tool 结果必须带 `tool_call_id` 追加进 messages；不回填，模型就无法基于观察继续推理，只会重复调用同一个工具。
- **4 个工具等于无边界（易错点）**：文件写 + Shell + 代码执行三件套到手，Agent 在本机真能「为所欲为」；上线前必须收敛权限、加沙箱与审计。
- **把框架做厚（易错点）**：复杂组件库会遮住 Agent 的本质；先能跑通 279 行的最小闭环，再按自己的失败模式加组件，而不是反过来。
