---
title: "如何用Prompt技巧激发无限创意"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247540408&idx=1&sn=3181257fb4f8fc393b50c1a1943c6ded"
source: "微信公众平台"
published: "2024-08-23"
archived: "2026-11-01"
category: "AI应用"
tags: [AI应用, Prompt]
lang: zh
status: ok
阅读页: "阅读页/AI应用/如何用Prompt技巧激发无限创意.html"
---

# 如何用Prompt技巧激发无限创意

> 一句话定位：系统梳理官方与大厂的Prompt技巧，并用Img2code实战验证

## 摘要

本文系统梳理当前主流的 prompt engineering 实践。先回顾新一代大模型让 prompt 长度与复杂度限制放宽的背景；接着总结 ChatGPT 官方文档的 6 条建议：指令清晰详细、问题含细节、角色扮演、使用分隔符、指定步骤、提供示例、设定回答长度，并补充参考文本、拆分复杂任务、给模型时间思考、使用外部工具与系统测试等技巧；再对比 Anthropic 强调评估优先与 MetaPrompt 的优缺点，以及 Google 的 Persona/Task/Context/Format 四要点；随后以 Img2code 任务演示从朴素 prompt 到 CO-STAR、COT、Reflection 的逐级优化效果；最后给出提示词网站与自动优化工具，并指出模型智能是基础，Prompt 是锦上添花。

## 要点

- 本文汇总 OpenAI、Anthropic、Google 官方资料与开源社区的 prompt 技巧
- 官方 6 条建议强调指令清晰、细节充分、角色扮演、分隔符、步骤与示例
- 参考文本、拆分任务、让模型思考、外部工具与系统测试构成完整方法论
- Anthropic 主张评估优先，特色是预填输出、数据指令分离与 MetaPrompt
- Google 以 Persona/Task/Context/Format 四要点提供对新手友好的教程
- Img2code 实战显示朴素 prompt 效果差，CO-STAR/COT/Reflection 逐级改善
- 模型智能是 0→60 分基础，Prompt 是 60→90 分的锦上添花

## 提炼

> 一句话定位：系统梳理官方与大厂的Prompt技巧，并用Img2code实战验证

### 分节提炼

- **01 背景**：新一代模型如 GPT-4、Gemini 理解上下文能力显著提升；prompt 长度和复杂度限制越来越宽泛；本文结合 OpenAI、Anthropic、Google 资料解析实践策略
- **02 Prompt原则与技巧**：官方 6 条建议：指令清晰、含细节、角色扮演、分隔符、步骤、示例、定长度；复杂任务应拆分为简单步骤，并让模型先思考再下结论；用嵌入检索实现 RAG，用 API 扩展模型能力；系统测试需建立指标评估、模型打分与人工评估流程
- **03 大厂方案**：Anthropic 强调把精力放在评估指标与鲁棒测试用例而非编写 prompt；Anthropic 特色技巧：预填模型输出、数据指令分离、MetaPrompt 工具；Google 四要点：Persona、Task、Context、Format，教程对新手友好
- **04 实践示例**：Img2code 任务分内容理解与代码输出两部分；直接 instruct GPT-4o 生成的页面不成形；按 Prompt 原则优化后接近小程序，CO-STAR、COT、Reflection 进一步改善
- **05 进一步**：可借助 aishort.top、Awesome ChatGPT Prompts 等现成提示词库；PromptPerfect 可自动优化已写好的 Prompt；模型本身智能是基础，Prompt 更多是锦上添花
- **06 写在结尾**：感谢 Superapp AI 项目研发组同事的共同努力与创新；希望文章激发更多创意，助力大模型应用探索

### 带走三句话

1. 写 Prompt 先给清晰指令与足够细节，必要时指定角色、分隔符、步骤和示例。
2. 复杂任务要拆分、给模型思考空间，并用外部工具与系统测试保证质量。
3. 模型智能是基础，Prompt 技巧是锦上添花，别只堆技巧而忽略模型能力。

### 知识卡

跨全文挖掘 10 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
