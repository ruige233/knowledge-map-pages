---
title: "Loop engineering：把 agent 放进工程循环"
url: "https://mp.weixin.qq.com/s?__biz=MzAxNDEwNjk5OQ==&mid=2650546258&idx=1&sn=3b972197775c805cb4a58bdfdf69618a"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent框架, Agent]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/Loop engineering：把 agent 放进工程循环.html"
---

# Loop engineering：把 agent 放进工程循环

> 一句话定位：把 agent 放进工程循环：状态、验证、权限、停止条件缺一不可

## 摘要

本文讨论 Loop engineering：不再逐轮提示 agent，而是设计一套自动循环系统，按固定节奏读取外部状态、判断下一步、执行任务、验证结果、写入状态并判断停止条件，工程师从每轮操作者变为循环机制的设计者与最终责任人。文章拆解支撑该系统的六大组件：负责唤醒的 automation、隔离并行的 worktrees、沉淀项目知识的 skills、接入外部工具的 MCP 与插件、读写分离的子 agent，以及持久化状态的记忆层。作者强调 loop 不是盲目追求全自动，重点在控制面：稳定输入、可 review 输出、明确验证、分层权限、可恢复失败与可沉淀产物，并用预算约束 token 成本。最终验收标准是 loop 是否让人更理解系统，而非更少读代码、更少担责。

## 要点

- Loop engineering 是把 agent 放进持续工程流程，工程师从逐轮操作者变为循环机制设计者与最终责任人。
- 一个能工作的工程 loop 至少六个动作：读取外部状态、判断下一步、执行任务、验证结果、写入状态、判断停止条件。
- 少了状态写入只是一次会话，少了停止条件就是烧 token 的定时器。
- harness 是控制件（guides 事前喂规则、sensors 事后验证修正），loop 是调度与闭环，两者缺一不可。
- 六大组件：automation 唤醒、worktrees 隔离并行、skills 沉淀知识、MCP/插件接入外部工具、子 agent 读写分离、memory 持久化状态。
- 按风险分层授权：observe 只读、propose 只提案不合并、act with approval 需批准、autonomous 只给低风险可回滚且验证强的任务。
- 可落地的 loop 要满足：输入稳定、输出可 review、验证明确、权限可控、失败可恢复、产物可沉淀。
- 验收标准：这个 loop 是让你更理解系统，还是让你更少读代码、更少判断、更少承担责任。

## 提炼

> 一句话定位：把 agent 放进工程循环：状态、验证、权限、停止条件缺一不可

### 分节提炼

- **01 全文**：定义 loop：读状态、判任务、执行、验证、写状态、判停止六动作；区别于 prompt/context/harness engineering。；六大组件各司其职：automation 唤醒、worktrees 并行隔离、skills 把稳定知识移出上下文、MCP/插件接工具、子 agent 读写分离、memory 落盘。；控制面优先：feedforward 给约束、feedback 给传感器，权限分层，写查分离，验证结果结构化并设预算与停止条件。；落地示例：CI triage loop、review comment loop、harness maintenance loop，均强调边界清楚、输出可 review。

### 带走三句话

1. 别再逐轮提示 agent，去设计有状态、有验证、有权限与停止条件的工程 loop。
2. 控制面比自动化程度重要：写清 feedforward/feedback，按风险分层授权，写的人和查的人分开。
3. 用“这个 loop 是否让你更理解系统”验收：更少读代码、更少担责的自动化只是更快地欠债。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
