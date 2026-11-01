---
title: "AI Agent 面试参考题二"
url: "https://mp.weixin.qq.com/s?__biz=MzkzMTIzMTI1Ng==&mid=2247483920&idx=1&sn=6b32b621acb6a199796724c68b1fb306"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "面试"
tags: [Agent, 面试]
lang: zh
status: ok
阅读页: "阅读页/面试/AI Agent 面试参考题二.html"
---

# AI Agent 面试参考题二

> 一句话定位：Agent Harness 运行时工程与框架选型面试题精讲

## 摘要

本文是 AI Agent 面试参考题第 24~36 题，聚焦 2026 年兴起的 Agent Harness 与运行时工程。Prompt/Context/Harness 三代演进，模型过阈值后运行时收益大于提示词调优。Harness 是套住模型的运行时壳，与 LangChain 式编排互补。选框架看状态显式、失败可解释、可恢复。LangGraph 靠 super-step 快照与 thread_id 版本链中断恢复。执行链降级按能力层级设计，坚持幂等优先与降级可观测。

## 要点

- Prompt/Context/Harness 是三代演进：管说什么、管给什么看、管怎么干。
- 模型能力过阈值后，上下文与 harness 收益远大于 prompt 调优，生产看复合成功率。
- Harness 是 Agent Engineering 的运行时底座，与 LangChain 式编排框架互补而非替代。
- Claude Code 用缓存、卸载、压缩、子 Agent 隔离、长期记忆五层机制管上下文。
- 生产选框架看状态显式、失败可解释、可恢复，而非 demo 惊艳。
- 平台侧用统一契约加适配器、网关、注册中心收敛异构 Runtime。
- LangGraph 以 super-step 快照、thread_id 版本链实现中断恢复与 time travel。
- 执行链降级按能力层级设计，坚持幂等优先、预算全局化、降级可观测、兜底诚实。

## 提炼

> 一句话定位：Agent Harness 运行时工程与框架选型面试题精讲

### 分节提炼

- **01 开篇：Harness 方向**：考的是理解力而非背诵，要体现为什么现在才火；覆盖第 24~36 题，共 13 道面试题
- **02 三层工程区别**：三代演进：Prompt 管指令、Context 管 token、Harness 管运行时；模型超阈值后，上下文与 harness 收益远大于 prompt 调优；生产看几百次调用的复合成功率
- **03 Agent Engineering vs Harness**：Agent Engineering 是方法论全流程，Harness 是运行时设施；Harness 是 Agent Engineering 最重最硬的工程产出
- **04 harness 为何火**：三个原因：重心转移、Chain 变工具循环、横切关注点变多；LangChain 管怎么拼，Harness 管怎么管住
- **05 新范式与论文**：Harness 内核：解耦、可恢复、薄抽象；Hermes 强调自我描述、自我修正与消息契约；论文按推理/技能/工具/记忆/评测五条线索回答；Hermes 是产品，Harness 是支撑它的基础设施
- **06 主流框架盘点**：Python 九大框架，LangGraph 是多 Agent 主流；TS/JVM 各有对应，Claude Code 是 harness 标杆；选型三句话：显式状态机、企业治理、读代码学内核
- **07 Claude Code 上下文管理**：五层机制：缓存、卸载、压缩、隔离、长期记忆；卸载优先于压缩，前缀稳定换缓存，隔离优于共享；压缩保留目标、事实、待办三类结构化信息
- **08 LangGraph vs Deep Agents**：LangGraph 胜在显式状态机、可恢复、HITL、生态；Deep Agents 不稳：黑盒多、隐式压缩、迭代快、错误不透明
- **09 Runtime 统一适配**：收敛到一层抽象，差异下沉到适配器；统一接口/网关/注册中心/状态/可观测/能力路由七项；验收：新增 Runtime 只写 Adapter，业务零改动
- **10 Super 还是 Host+Sub**：长期是强 Host 加窄 Sub 加工具生态的混合形态；Sub 本质是上下文隔离，权限合规必须分域；终局哑铃型，半专业 Agent 层消失
- **11 LangGraph 状态快照**：LangChain 解决怎么拼，LangGraph 解决怎么跑与恢复；快照粒度是 super-step，thread_id 串版本链；恢复从 checkpoint 的 next 节点续跑，不重放历史
- **12 Java 选 Spring AI**：与 Spring 生态原生融合，接入成本几乎为零；统一抽象切换模型只改配置，治理能力齐备；Spring AI Alibaba 补齐 Graph 编排与国内适配
- **13 非 Java 生态框架**：Python 主流最多，Go 有 Eino，Rust 有 Rig；n8n/Dify/Coze 等低代码平台业务侧常用；回答策略：挑 2~3 个讲差异而非罗列名字
- **14 执行链与降级**：执行链分层：校验、路由、装配、Loop、汇总、观测；降级按能力层级设计，七类失败各有三级兜底；幂等优先、预算全局化、降级可观测、兜底诚实

### 带走三句话

1. 生产系统真正的指标是复合成功率，而非单次调用成功率。
2. 选框架看状态显式、失败可解释、能否恢复，别被 demo 迷惑。
3. 降级要分层设计：幂等优先、预算全局化、兜底诚实可观测。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
