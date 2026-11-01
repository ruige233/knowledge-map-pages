---
title: "AI Agent 面试参考题一"
url: "https://mp.weixin.qq.com/s?__biz=MzkzMTIzMTI1Ng==&mid=2247483916&idx=1&sn=e26917153d74d48532c30e642d972ce5"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "面试"
tags: [Agent, 面试]
lang: zh
status: ok
阅读页: "阅读页/面试/AI Agent 面试参考题一.html"
---

# AI Agent 面试参考题一

> 一句话定位：Agent 面试真题：概念边界、架构模式与落地工程要点速览

## 摘要

本文是 AI Agent 面试参考题第一辑，收录第 2~20 题。核心定义：Agent 以 LLM 为推理内核，通过工具调用在感知、规划、行动、观察循环中自主推进；与传统对话、Siri 的边界是行动闭环与能力动态组合。文章厘清 Agent 与 Workflow 关系，给出单次提示词、Workflow、Agent 三档选型依据，梳理 ReAct 等模式，并总结分层架构、状态外置、工具治理等工程要点。最后落到成本：主 Agent 强模型、子 Agent 中小模型可省 50%~70%。

## 要点

- Agent 是以 LLM 为推理内核，通过工具调用在「感知→规划→行动→观察」循环中自主推进直到达成目标的系统。
- 与传统对话、Siri 的边界在于「行动—观察—再决策」闭环与能力动态组合，能力边界是「可验证、可回滚」。
- Workflow 是人写死控制流，Agent 是模型生成控制流；能用 Workflow 解决就别上 Agent。
- 常见架构模式有 ReAct、Plan-and-Execute、Reflexion、Router、Supervisor-Worker、Pipeline、Debate、分层混合。
- ReAct 实现简单但短视、token 消耗高；Plan-Execute-Replan 有全局视野，需监管 Agent 做校验、偏离检测、重规划与兜底。
- Agent 项目采用基础设施、能力、编排、接入四层架构；服务无状态、状态外置，长任务队列异步，金丝雀发布。
- 企业落地最大瓶颈是可靠性、可评估性与组织配套；多步复合成功率指数衰减。
- 模型选型按职责分层：主 Agent 用强模型、子 Agent 用中小模型，可省 50%~70% 成本。

## 提炼

> 一句话定位：Agent 面试真题：概念边界、架构模式与落地工程要点速览

### 分节提炼

- **01 基础概念与架构模式**：覆盖第 2~20 题，主题是 Agent 基础概念、边界与架构模式；题目来源多为字节、阿里、腾讯、美团等大厂
- **02 什么是 Agent 及其边界**：Agent 是能自己决定下一步并动手执行的程序；边界看两点：有无行动闭环、能力是枚举还是组合
- **03 为什么用 Agent**：Agent 定义为 LLM 推理内核加工具调用，在感知规划行动观察循环中推进；裸模型三约束：知识截止、不能产生副作用、不能一步到位
- **04 Agent 与 Workflow 关系**：Workflow 是人把控制流写死，Agent 是模型把控制流生成出来；判断四问：步骤可预知、分支可穷举、中间结果依赖、错误代价
- **05 常见架构模式**：八种模式：ReAct、Plan-Execute、Reflexion、Router、Supervisor、Pipeline、Debate、分层混合；选型原则：控制流越确定越靠 Workflow，目标越开放越靠 Supervisor+ReAct
- **06 ReAct 原理与优缺点**：ReAct 即 Reasoning+Acting，按 Thought→Action→Observation 循环到 Final Answer；优点：实现极简、逐步利用观察、过程可读；缓解：最大轮次、重复调用检测、阶段性重述目标
- **07 ReAct 与 Plan-Execute-Replan**：ReAct 每步现想现做、局部短视；Plan-Execute-Replan 先全局计划、可审查可干预；复杂 Agent 通常 Plan 阶段定骨架，每步内部用 ReAct 执行
- **08 为何需要监管 Agent**：监管 Agent 负责计划合理性校验、执行偏离检测、重规划触发、收敛兜底；Planner 输出是一次性假设而执行环境会变，需监管兜底
- **09 项目架构与部署**：四层架构：基础设施、能力、编排、接入；框架选型：JVM 用 Spring AI，Python 用 LangGraph 或轻量自研 harness；服务无状态、状态外置，长任务队列异步，金丝雀发布
- **10 Agent 与大模型区别**：大模型提供能力是原料，Agent 提供结果是产品；模型侧关心效果上限，Agent 侧关心工具成功率、循环控制等工程可靠性
- **11 与传统后端差异**：控制流、输出、延迟、成本、错误处理、测试、可观测等维度都不同；最被低估的差异是延迟和成本的一次性放大
- **12 三种方案选型**：按「不确定性 × 步骤数」两轴选型；决策顺序：先单次提示词，不行再 Workflow，再不行才 Agent
- **13 固化还是保泛化**：高频、路径稳定场景固化成 Skill 或固定 Workflow，长尾探索保泛化；成熟形态是双轨制：通用 Agent 兜底加 Skill Router 路由专用链路
- **14 最小可用 Agent 模块**：ReAct Loop 加工具只是骨架，还需预算、防环、治理、持久化、可观测、降级、安全；Loop 是心脏，预算、终止、治理、可观测是上线的血管
- **15 落地瓶颈与弊端**：最大瓶颈是可靠性加可评估性加组织配套，不是模型能力；多步复合成功率指数衰减，单步 95% 十步只剩 60%
- **16 Agent 与 Siri 差异**：差异在意图来源、能力组织、执行模式、状态记忆、可扩展性五方面；根本差异：Siri 流程人写好，Agent 流程模型运行时生成
- **17 兼顾全局规划与实时反馈**：Supervisor 下 Planner 拆 DAG，Executor 内部跑 ReAct，Critic 校验，失败局部重规划；计划粒度适中，经验值每步 3~8 次工具调用；共享上下文分全局区与局部区，统一中断语义
- **18 通俗解释 Agent**：比喻：大模型是只能说话的顾问，Agent 给顾问配上手脚和眼睛能办事；区别像照菜谱做菜与请厨师：Agent 说清目标自己想办法
- **19 底层模型选型**：选型七维度：任务能力、工具调用、上下文、延迟吞吐、成本、可控性、稳定性；主 Agent 用强模型定方向，子 Agent 用中小模型加专用 Prompt 省成本；分层通常省 50%~70% 成本且端到端成功率几乎不掉
- **20 Agent 类型划分**：按认知分工：Planner、Executor、Critic、Router、Reflector、Memory Manager；还可按系统形态、自主程度、技术路线划分

### 带走三句话

1. Agent 的边界是「可验证、可回滚」；无法验证或回滚的场景必须加人工确认 HITL。
2. 选型顺序：单次提示词 → Workflow → Agent，路径不确定时才为灵活性付成本。
3. 生产可用 Agent 不止 ReAct Loop，预算、终止、治理、可观测四件事缺一不可。

### 知识卡

跨全文挖掘 12 张知识卡（数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
