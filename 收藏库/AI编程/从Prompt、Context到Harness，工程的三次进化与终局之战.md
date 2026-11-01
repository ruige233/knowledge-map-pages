---
title: "从Prompt、Context到Harness，工程的三次进化与终局之战"
url: "https://mp.weixin.qq.com/s?__biz=MzI2NDU4OTExOQ==&mid=2247695574&idx=1&sn=5a72299143649e58cc510ab1b50480b4"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI编程"
tags: [AI编程, Harness, Prompt]
lang: zh
status: ok
阅读页: "阅读页/AI编程/从Prompt、Context到Harness，工程的三次进化与终局之战.html"
---

# 从Prompt、Context到Harness，工程的三次进化与终局之战

> 一句话定位：Prompt说清楚、Context给够信息、Harness让系统可靠，三者嵌套。

## 摘要

OpenAI 3-7 人团队 5 个月生成近 100 万行代码，引出三次进化。Prompt Engineering 用精心输入激发模型能力，模型变强后效益降低。Context Engineering 用 RAG、上下文压缩、单一事实来源管理信息注意力。Harness Engineering 设计马具，用上下文治理、验证闭环、技术债清理与 F-Harness 分工，20 倍时间、22 倍成本换生产级质量。三者嵌套而非替代，模型越强所需 Harness 越简单，工程师转向定方向、搭架子、做判别。

## 要点

- OpenAI 3-7 人团队 5 个月让 AI 生成近 100 万行生产级代码，工程师未手写业务逻辑。
- Prompt Engineering 本质是“加约束”，用精心设计的输入激发模型正确能力，但随模型变强边际效益降低。
- Context Engineering 把模型当作“金鱼助理”，用 RAG、上下文压缩、单一事实来源解决信息注意力问题。
- Prompt 与 Context 的共同盲区是缺少系统级约束、验证和反馈机制。
- Harness 三大策略：上下文治理、验证闭环、技术债清理；F-Harness 三 Agent 分工。
- 三者是嵌套关系而非替代：Prompt 说清楚、Context 给够信息、Harness 保证系统可靠。
- “Harness 衰变定律”：模型能力越强所需 Harness 越简单，应动态调整 Harness 的厚度。
- 工程师职责重构为定方向、搭架子、做判别，衡量标准从个人产出转向系统杠杆。

## 提炼

> 一句话定位：Prompt说清楚、Context给够信息、Harness让系统可靠，三者嵌套。

### 分节提炼

- **01 引言：百万行代码冲击**：以 OpenAI 3-7 人团队 5 个月生成近 100 万行代码抛出问题；答案指向 Prompt、Context、Harness 三次进化
- **02 Prompt：把话说清楚**：Prompt Engineering 本质是通过加约束激发模型的正确能力；武器库：零样本、少样本、思维链、角色扮演、提示链；模型语言理解力变强后，写好 Prompt 的边际效益显著降低
- **03 Context：给够信息**：用“金鱼助理”思想实验理解 Context：每次会面前整理简报；模型只能看到上下文窗口内的信息，窗口外一无所知；RAG 不存知识存索引，按需检索精准注入；上下文压缩对抗遗忘；单一事实来源保证 AI 信哪个版本
- **04 共同盲区**：Prompt 和 Context 到位后，Agent 仍会跑偏、重复犯错；问题根源在系统层面缺乏约束、验证和反馈机制
- **05 Harness：驾驭 AI 的马具**：Harness 即马具：大模型之外的一切系统设计；OpenAI 三大策略：上下文治理、验证闭环、技术债清理；F-Harness 用 Planner/Generator/Evaluator 分工对抗自评乐观
- **06 三者嵌套**：三者是层层包裹、相互依存的嵌套关系，不是替代；三个问题划清边界：说什么、该知道什么、系统如何可靠运转
- **07 衰变定律**：模型能力越强，所需 Harness 越简单；Harness 是当下现实答案，但可能是过渡性技术；把精力集中在业务逻辑边界与外部环境接口
- **08 人掌舵、Agent 执行**：工程师核心职责：定方向、搭架子、做判别；衡量标准从个人产出切换为系统杠杆
- **09 四步实践路线**：四步：打牢 Prompt 基础、学 Context、系统视角设计、动态 Harness 思维；动态思维：判断约束是因模型能力不足还是业务本身需要
- **10 结语：三次进化一个目标**：工程师价值在搭建的 Harness 系统，而非手写代码速度；软件工程在进化：从写代码的人到设计让 AI 把代码写好的人

### 带走三句话

1. 工程进化的终点不是更强的模型，而是更可靠的系统：Harness 决定 AI 能否在生产环境持续产出。
2. 按模型能力边界动态调整 Harness 的厚度：不过度设计，也不遗漏关键约束。
3. 工程师的新竞争力从写代码转向设计让 AI 把代码写好的系统，衡量标准是系统杠杆。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
