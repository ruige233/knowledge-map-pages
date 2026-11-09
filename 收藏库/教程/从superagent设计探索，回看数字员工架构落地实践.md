---
title: "从 SuperAgent 设计探索，回看数字员工架构落地实践"
url: "https://mp.weixin.qq.com/s/r72ytLABzyf7ktUSgMseTg"
source: "微信公众平台 · Qoder"
published: "2026-11-06"
archived: "2026-11-09"
category: "教程"
tags: [Agent 研发, Agent 编排, 上下文工程]
lang: zh
status: ok
阅读页: "阅读页/教程/从superagent设计探索，回看数字员工架构落地实践.html"
---

# 从 SuperAgent 设计探索，回看数字员工架构落地实践

> 一句话定位：以数字员工 QoderWake 为例，讲清「消息持续变化时，一件工作如何继续推进」的路由、上下文与权限设计。

## 摘要

本文从 DeepSeek Harness 团队发布的《Super-agent 设计探索》出发，回看数字员工 QoderWake 在 IM 场景下的架构实践。作者指出：Agent 执行完任务只是第一步，协调工作（消息归属、补充要求、求助反馈、结果回流）才是真正消耗人注意力的部分。QoderWake 的解法是把消息路由与具体执行分开——Task Router 判断意图、关联任务、选择执行者，Task Coordinator 以程序方式管理锁、队列、准入与生命周期，Worker 在隔离上下文推进任务；Context 按角色分成六组（Router 用候选任务与近期摘要，Worker 用目标/结论/本轮要求，专家只给必要背景），并区分「排查材料」与「任务指令」的作用。主动响应经过程序检查、轻量初筛、Router 确认三层判断，权限约束落实到数据访问、工具调用与实际操作。内部评测显示：QoderWake 多轮连续性 86.56 分，普通 Waker 为 0.00 分。结论是让 Agent 持续工作、让人回到判断与决策。

## 要点

- 分工：Task Router（意图/关联/选执行者/轻量直答）与 Work 层（具体任务）分离；Task Coordinator 是程序实现的运行控制组件，负责锁、队列、准入、状态、取消与恢复，不是第三个推理 Agent。
- 三个断点：消息与任务能否关联、协作之后能否接续、结果能否回到原处——单轮回答质量无法覆盖这些多轮连续性问题。
- Context 分组：Router 看候选任务与近期摘要，Worker 看当前目标/已有结论/本轮要求，专家只需足够回答问题的背景；遵循关联、分层、按需、隔离四原则。
- 主动响应三层判断：程序检查来源/时效/定向关系 → 轻量模型初筛（不规划不回复）→ Router 结合群策略确认职责归属；初筛不代表授权。
- 权限约束：分析原因、整理清单、修改配置所需权限不同；理解问题不等于获得授权，讨论中有人提建议也不能当作授权。
- 数据：300 个脚本案例评测，多轮连续性 QoderWake 86.56 vs 普通 Waker 0.00（相差 86.56 分）；但及时性 QoderWake 低近 7 分，差异不能全归因于 Router。

## 关联

- 《一文拆解DeepSeek Harness》（`收藏库/教程/一文拆解deepseek-harness.md`）——本文回看的 SuperAgent 设计探索原文解读。
- 《从OpenClaw，怎么走到了Super Agent？》（`收藏库/AI应用/从OpenClaw，怎么走到了Super Agent？.md`）——同一「从 Agent 到 Super Agent」的演进话题。
- 《多Agent协作的下一场竞争，不在模型，而在上下文》（`收藏库/AI应用/多agent协作的下一场竞争，不在模型，而在上下文.md`）——上下文如何流动与组织，本文的 Context 分组是其中一种落地。

## 提炼

（待子 Agent 返回后追加）
