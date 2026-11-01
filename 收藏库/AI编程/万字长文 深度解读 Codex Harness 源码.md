---
title: "万字长文  深度解读 Codex Harness 源码"
url: "https://mp.weixin.qq.com/s?__biz=MjM5NjczNjMwMQ==&mid=2247489694&idx=1&sn=9c96d8908f6d481077f4de411fc5a54e"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI编程"
tags: [AI编程, Harness]
lang: zh
status: ok
阅读页: "阅读页/AI编程/万字长文 深度解读 Codex Harness 源码.html"
---

# 万字长文  深度解读 Codex Harness 源码

> 一句话定位：拆解 OpenAI Codex Harness 源码，讲清 Agent 运行时实现。

## 摘要

本文沿 OpenAI 开源的 Codex Harness 源码（commit 633ab199cf）逐层拆解 Agent 运行时架构。先厘清 Host、Harness、Model 三层边界，以及 Thread/Turn/Step/Item 四种上下文粒度；再从 session/turn_input.rs 的任务准入、run_turn 主循环、ToolRouter 与 ToolCallRuntime 的工具调度、沙箱与审批的分层安全、历史压缩与 Rollout 恢复、app-server 双向 JSON-RPC 控制面，到 MCP/Skills/Plugins/多 Agent 扩展，完整还原一条任务从接收到执行的事件化过程。最后归纳六条设计思想，并给出接入产品时的职责边界、五个要点与适用场景。结论：Harness 是把模型决策转化为可执行、可控制、可恢复、可观察任务的运行时，而非提示词模板。

## 要点

- Harness 是可复用中间层：Host 负责外壳与业务，Model 只提建议，真正把建议转为受控动作的是 Harness。
- 核心对象按粒度分层：Thread 是长期对话、Turn 是一次任务、Step 是一次采样快照、Item 是可持久化事实。
- run_turn 是 Agent 主循环：采样→工具执行→结果回填→按 needs_follow_up 决定继续或结束，而非单次调用。
- StepContext 在每次采样前冻结工具、环境与权限，保证模型“看见什么”与运行时“允许什么”一致。
- ToolRouter 对齐可见规格与真实 handler，ToolCallRuntime 用读写锁实现并发、独占与取消。
- 安全分层叠加：工具 schema、ToolRouter、沙箱、执行策略与审批；审批通道失效默认 Abort（fail closed）。
- 历史与 Rollout 持久化让任务可恢复、可审计；工具结果必须写回模型上下文才能避免“刚做完就忘”。
- app-server 以双向 JSON-RPC 作控制面，UI 只订阅 Thread/Turn/Item 事件，实现界面可替换。

## 提炼

> 一句话定位：拆解 OpenAI Codex Harness 源码，讲清 Agent 运行时实现。

### 分节提炼

- **01 概念分层**：Host 是外壳，Harness 是可复用中间层，Model 只输出建议。；Thread 是协议层长期对象，Core 内运行时对象叫 Session。；一个 Turn 需多次采样，Step 是必须存在的中间快照。；工具闭环：模型产生 FunctionCall，Harness 执行并写回 FunctionCallOutput。
- **02 仓库结构**：Harness 主体在 codex-rs，主线集中在 core、tools、protocol、app-server。；codex-rs 众多 crate 是在拆开协议、存储、沙箱等系统边界，便于独立演进。；读源码路线：README→turn_input→tasks→run_turn→tools→审批。
- **03 运行机制**：turn_input 判定启动/转向(steer)/拒绝，spawn_task 变成带取消令牌的后台 SessionTask。；run_turn 循环捕获 StepContext 采样，工具回填后按 needs_follow_up 续跑或结束。；ToolRouter 对齐可见规格与 handler；读写锁区分并发/独占并支持取消。；历史与 Rollout 支撑恢复、分叉、审计；扩展都走 StepContext 主链。
- **04 设计思想**：把工作建模为 Thread/Turn/Step/Item，而非 ask_model 单次调用。；StepContext 是决策与执行同源的一次交易快照。；Item 让行动结果成为下一步推理证据，避免“刚做完就忘”。；提示词建议下一步，状态机收敛事实生效与异常；安全分层划定边界。
- **05 接入方法**：业务系统仍是 system of record，Harness 不是数据库。；五个要点：结构化上下文、显式写工具、展示 Item、保留取消重连、先定风险边界。；简单文本转换直接调模型；多步行动与真实状态变更才用 Harness。
- **06 常见问题**：Harness 不是 ReAct，是把该模式落到生产运行时的完整工程系统。；Harness 不改变模型权重，但影响长任务上下文、工具与失败恢复。；开源仓库不含模型，模型访问与托管服务独立。；三个入口：run_turn、app-server turn/start、ToolRouter 与审批。
- **07 总结**：三句话：它是循环、它是运行时、它是集成层。；关键符号联想：run_turn=采样循环，StepContext=固定快照，ToolRouter=能力对应。

### 带走三句话

1. Harness 是把模型决策落地为可执行、可控制、可恢复、可观察任务的运行时，不是提示词模板。
2. StepContext 让模型所见与运行时所允许保持一致，工具结果必须回填才能成为下一步证据。
3. 接入产品时业务系统仍是 system of record，把写操作做成显式工具、风险边界交给权限与策略。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
