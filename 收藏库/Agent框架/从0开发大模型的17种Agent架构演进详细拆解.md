---
title: "从0开发大模型的17种Agent架构演进详细拆解"
url: "https://mp.weixin.qq.com/s?__biz=MjM5ODYwMjI2MA==&mid=2649801545&idx=1&sn=b2d15c583715f519cbec484e8b10abdf"
source: "微信公众平台 · 周末程序猿"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent框架, Agent, 大模型]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/从0开发大模型的17种Agent架构演进详细拆解.html"
---

# 从0开发大模型的17种Agent架构演进详细拆解

> 一句话定位：17种Agent架构的本质是控制流设计，可用agno复现

## 摘要

本文拆解 GitHub 项目 all-agentic-architectures 的 17 种 Agent 架构，并用 agno 框架逐一复现。核心论断：Agent 架构的本质不是 prompt 工程或框架 DSL，而是控制流设计——状态建模、显式控制流、错误局部截断、副作用闸门与停止条件。文章给出统一分析框架（问题、State、拓扑、Router、失败模式、升级时机），并按演化路径梳理 Reflection、Tool Use、ReAct、Planning、PEV、多 Agent、记忆、ToT、模拟与涌现等，最后以架构演化表和按“缺哪种控制能力”的选择表收尾。

## 要点

- Agent 架构的本质是控制流设计，而非 prompt 工程或框架 DSL，可在任何体面框架中复现。
- 决定系统能否落地的关键：状态建模、显式控制流、错误局部截断、副作用闸门、知道何时该停。
- 统一用六问拆解每种架构：解决什么问题、State、拓扑、Router、失败模式、何时升级。
- 演化主线：单次生成→反思闭环→工具交互→观察行动循环→显式规划→验证驱动重规划→多 Agent→记忆→搜索/模拟/涌现→可信任。
- agno 的 Agent、Workflow、Step、Router、Loop、Parallel 等抽象足以表达几乎全部架构。
- 选型应问“缺哪种控制能力”，而不是“哪个架构好”。
- evaluator 是闭环系统的核心而非附属，决定何时停、何时改、何时拒绝。
- 鉴别新架构名词三问：新增什么 state、什么 router、什么 evaluator。

## 提炼

> 一句话定位：17种Agent架构的本质是控制流设计，可用agno复现

### 分节提炼

- **01 统一分析框架**：六问覆盖问题、State、拓扑、Router、失败模式与升级时机；后续不描述“更聪明”，只描述新增的状态、路由与验证
- **02 用agno看Agent**：response_model定义状态空间，Agent/tools/Step定义状态变换；steps定义确定性转移，Router定义条件转移，Loop.end_condition定义终止；跨非相邻步骤用workflow_session_state显式传状态
- **03 总览表**：17种架构各自新增一种控制能力，并解决一个老问题；演化目标不是AGI，而是更复杂环境下可控、可解释、可恢复
- **04 Reflection**：把单次生成拆成generator+critic+refiner三阶段；核心state字段是draft/critique/refined_code；没有router，三步走完直接结束
- **05 Tool Use**：state变成框架托管的会话事件日志；控制流从文本跨越到结构化世界再返回；底层是内置while循环：有tool_calls就继续
- **06 ReAct**：消息序列升级为行动轨迹trace，观察驱动下一轮决策；tool→model回边把单次调用变成持续交互；覆盖80%任务：先看一眼、做动作、按反馈再决策
- **07 Planning**：把未来控制流提前materialize成Plan数据结构；路由逻辑变成“计划是否还有剩余项”；计划可可视化、检查、修改、追踪进度
- **08 PEV**：执行视为生成待验证结果，verifier输出结构化verdict；失败被局部识别并重定向回re-plan路径；router在continue/replan/finish间决策
- **09 Multi-Agent**：解决单prompt角色冲突，把认知分工编码到架构；state按角色分字段，体现角色边界；可用Workflow或Team(mode=coordinate)实现
- **10 Blackboard**：共享黑板当前状态决定下一步激活谁；controller每次看黑板决定next_agent或FINISH；控制中心从预定义工作流转向共享状态+调度器
- **11 Meta-Controller**：入口分诊，controller只做选择不做任务；与Blackboard区别：一次性路由vs持续调度；生产起步架构通常先选Meta-Controller
- **12 Ensemble**：fan-out/fan-in：多agent并行分析同一问题再由aggregator融合；用冗余与多视角降低单次推理偏差；关键不是取平均，而是保留并解释冲突信息
- **13 双记忆系统**：state扩展为图内状态+图外可检索历史；episodic记事件入向量库，semantic记事实入图/结构化存储；enable_agentic_memory自动接上写回链路
- **14 Graph Memory**：从chunk相似性召回升级到关系结构推理；链路为Text→KG→Text-to-Cypher→Query→Answer；适用于企业问答、组织关系、并购链条追踪
- **15 ToT**：把推理问题转写成候选路径空间的搜索问题；LLM负责生成候选动作，程序化代码负责搜索与剪枝；state单位变成多条候选路径active_paths
- **16 Mental Loop**：用copy.deepcopy做模拟分叉，预演后再决定是否真执行；决策agent通过simulate_action/execute_action双工具；反事实执行：先问“如果我这么做会怎样”
- **17 Dry-Run**：工具带dry_run参数，preview与execute两种模式；Workflow显式插入人工审批step做最后闸门；让副作用在控制流上变成可审批对象
- **18 Metacognitive**：维护自我模型，先问“这个任务我该不该做”；按confidence和策略路由reason_directly/use_tool/escalate；最强能力不是回答而是拒绝
- **19 Self-Improvement**：生成→评估→修订循环，不达标继续，最多修订3次；GoldStandardMemory把批准样本注入未来生成；与Reflection区别：跨任务积累高质量样例
- **20 Cellular Automata**：范式从中心控制转向分布式涌现；每个格子只跑局部规则，全局行为从局部交互涌现；LLM退居规则设计者/解释者位置
- **21 Evaluator**：五类评估器：LLM裁判、内置critic、程序化验证、HITL、演示式验证；闭环系统没有evaluator就不知何时停改拒；没有evaluator的agent只是会循环的prompt
- **22 架构演化表**：一表对照17种架构的新增能力、解决问题与agno对应能力
- **23 如何选型**：选型看缺哪种控制能力，不看哪个架构好；按质量、交互、步骤控制、分工、长期状态、求解范式、安全边界七类对照
- **24 最终结论**：agent architecture是控制流设计史，不依赖某一框架；真实系统会长出Workflow/Router/Loop等同一组抽象；新架构名词三问：新增什么state/router/evaluator

### 带走三句话

1. 先别迷信万能 agent，先把状态和控制流画清楚。
2. 大多数系统从 ReAct 起步，但可靠系统一定引入验证、记忆和边界控制。
3. 真正高级的 agent 不是更敢做事，而是更知道什么时候不该做。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
