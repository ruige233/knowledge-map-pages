---
title: "从 ReAct 到 Agent Teams：一个工程师视角的 Agent 协作机制思考"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247562213&idx=1&sn=b5d701423e4bcb9c5775a7c6af19e234"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent框架, Agent]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/从 ReAct 到 Agent Teams：一个工程师视角的 Agent 协作机制思考.html"
---

# 从 ReAct 到 Agent Teams：一个工程师视角的 Agent 协作机制思考

> 一句话定位：从 ReAct 三环节出发，提出七机制 Agent Team 协作设计

## 摘要

文章指出 Agent 本质是 ReAct 循环（思考→行动→观察）的实现，能力由模型、工具与观察反馈决定，工程可优化行动与观察；大模型无状态，经验必须靠上下文管理持久化。多 Agent 协作应借鉴人类组织：当前 Leader-Worker 架构让 Worker 隔离、缺横向通信与协作语义。作者梳理业界五阶段探索（分类、分工、目标、经验、演化），指出各自孤立且普遍缺横向通信、启发式管理与 Mission 锚定，据此提出七机制 Agent Team，强调多 Agent 价值全来自协作机制。

## 要点

- Agent 本质是 ReAct（Thought→Action→Observation）循环的实现，观察提供接地反馈以抑制幻觉。
- Agent 能力由思考（模型）、行动（工具）、观察（环境反馈）三环节决定，工程着力点是行动与观察。
- 大模型无状态，经验必须外部持久化再经 context 喂回，上下文即记忆是当前唯一可靠路径。
- 当前 Leader-Worker 架构 Worker 完全隔离，只有调度语义，缺方案讨论、共识与动态重规划。
- 业界探索可串成“分类→分工→目标→经验→演化”生命周期链，但各自孤立、普遍缺三样东西。
- 真正 Agent Team 需七机制：Leader 兜底、启发式管理、讨论共识执行、横向通信、OKR、岗位宗旨、集体复盘。
- 多 Agent 价值 100% 来自协作机制本身，堆 Agent 数量不产生价值（EvoChamber 消融实验）。

## 提炼

> 一句话定位：从 ReAct 三环节出发，提出七机制 Agent Team 协作设计

### 分节提炼

- **01 Agent 本质即 ReAct 循环**：ReAct 核心循环是 Thought→Action→Observation，推理、行动、根据结果调整。；Observation 是外部反馈（API 返回、命令输出、文件内容），让推理锚定事实、抑制幻觉。；pi-agent 的 agent-loop.ts 约 600 行即完成完整循环，记忆压缩等只是上下文优化。
- **02 思考、行动、观察三环节**：思考由模型基座决定，是 Agent 智商上限，换更强模型任务表现直接提升。；行动由工具集决定，ALFWorld 中动作空间让成功率比纯推理高 34 个百分点。；观察须真实结构化，工具返回详细错误与修复建议远比一个错误码有用。；工程着力点是行动与观察：提供更好工具、返回更结构化结果。
- **03 无状态与上下文即记忆**：大模型每次调用独立前向计算、无状态，所谓记忆是把历史塞回 context。；人脑学习物理改写突触，模型参数不变，经验必须外部持久化再喂回。；持续学习未解决灾难性遗忘，上下文即记忆是唯一可靠工程路径。
- **04 多 Agent 是必然进化方向**：单 Agent 是完善一个“人”，未来必出现 Agent 间协作问题。；ReAct 成功来自对个体智能的正确抽象，协作可借鉴人类组织经验。；Agent 无情绪、即时响应，沟通成本低是优势；劣势是 context 有限、缺常识、幻觉。
- **05 Leader-Worker 架构的不足**：主流框架 manager 本质是调度器，无方案讨论、共识达成、动态重规划。；核心缺陷是 Worker 完全隔离、缺 peer-to-peer 横向信息流。；AgentTeams 解决了通信基础设施，但协作语义仍缺失。
- **06 业界五阶段探索链**：五阶段链：分类→分工→目标→经验→演化，环环相扣。；分类：五种组织形态无普适结构，灵活交互场景 Flat/P2P 更优。；分工：MetaGPT 用 SOP 约束防跑偏，Agent-Oriented Planning 动态重规划求准，但都自上而下。；OKR-Agent 单 Agent 递归、Co-Learning 只积累个体经验，Meta-Team 与 EvoChamber 证明价值来自协作机制。
- **07 七机制 Agent Team 设计**：Leader 四特征：深度参与方案、先讨论后执行、兜底能力、方案变更审查。；启发式管理：鼓励式引导激活正向对齐分布，命令式只激发 60% 能力。；启动三步走（Proposal→Feedback→Consensus）与 Worker 三种横向通信（广播/查询/求助）。；OKR 须 Worker 参与制定，岗位与 Mission 锚定方向，复盘沉淀团队资产。
- **08 与现有框架对比**：对比 CrewAI/AutoGen、Meta-Team 与作者机制：Leader 从分发器变为资深专家兜底。；任务启动改为讨论后产出 OKR，Worker 执行中 P2P 求助，进度 OKR+里程碑，复盘沉淀团队资产。；团队目标以 Mission+OKR 锚定，补齐现有框架缺失。
- **09 部落到生态的演化类比**：部落→城邦→企业→生态四阶段，当前 Agent 协作处于部落或城邦早期。；从简单分工逐步走向 Mission+OKR、集体复盘与多 Team 协作网络。
- **10 Agent 自主性五级分级**：L1 Copilot 到 L5 Autonomous Organization 五级递进。；当前处于 L3→L4 之间，Meta-Team/EvoChamber 标志 L4→L5 过渡开始。
- **11 结语**：ReAct 证明对人类智能抽象建模可行，多 Agent 协作应提取组织经验。；Agent 消除情绪摩擦和沟通瓶颈，机制以更高频率、更低成本运转。；学术界 2025 年已回应这些问题，Team 时代正在到来。

### 带走三句话

1. Agent 本质是 ReAct 循环：模型定思考上限，工具与观察是工程可发力处。
2. 大模型无状态，经验不落库等于白干；上下文即记忆是当前唯一可靠路径。
3. 多 Agent 价值 100% 来自协作机制，堆数量不如建讨论、通信、OKR 与复盘。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
