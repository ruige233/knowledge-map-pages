---
title: "AI工程范式的跃迁，一文讲透Context Engineering！"
url: "https://mp.weixin.qq.com/s?__biz=MzI2NDU4OTExOQ%3D%3D&mid=2247697260&idx=1&sn=bbda847cb4e895e1ea99caf5acec9923"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent框架, Context Engineering]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/AI工程范式的跃迁，一文讲透Context Engineering！.html"
---

# AI工程范式的跃迁，一文讲透Context Engineering！

> 一句话定位：一文讲透Context Engineering：上下文工程的完整落地指南

## 摘要

本文沿 Prompt→ReAct→Context Engineering 的演进，讲清上下文工程的必要性。Prompt 只优化单次调用，ReAct 让 LLM 能行动却只进不出。CE 以策展、最优、演化三词，视上下文窗口为内存，用 Lost in the Middle、Context Rot、O(n²) 三大约束证明堆砌有害，提出“最小 Token × 最高信噪比”。随后拆解七类要素、长任务三大技术与 ReAct 陷阱破解，以 Python 示例和五大反模式收束，落到 Harness 落地。

## 要点

- 三个范式是叠加关系：Context Engineering ⊇ ReAct ⊇ Prompt Engineering。
- 上下文窗口就是 LLM 的工作内存，推理时只能看到窗口内的信息。
- 三大物理约束——Lost in the Middle、Context Rot、O(n²) 注意力预算——证明信息堆砌有害。
- 最优上下文 = 最小 Token 数量 × 最高信噪比。
- 上下文七类要素：系统指令、用户提示、状态历史、长期记忆、检索信息、可用工具、结构化输出。
- 长任务靠三大技术破局：上下文压缩、结构化笔记、子 Agent 上下文隔离。
- ReAct 是行动框架、CE 是信息保障，二者缺一不可；CE 可工程化为 Harness 团队资产。

## 提炼

> 一句话定位：一文讲透Context Engineering：上下文工程的完整落地指南

### 分节提炼

- **01 演进路线**：三阶段演进：Prompt优化单次调用，ReAct赋予行动能力，CE管理信息生命周期。；ReAct每轮Thought/Action/Observation都追加进上下文，长任务越积越大。；三范式是叠加关系：CE ⊇ ReAct ⊇ Prompt Engineering。；2025年业界共识：多数Agent失败是上下文失败，而非模型失败。
- **02 定义与基础**：Anthropic定义三关键词：策展、最优、演化；上下文持续动态更新。；上下文窗口就是工作内存，Context Engineering即LLM OS的内存管理。；英文1 Token≈4字符，中文1 Token≈1.5-2汉字；窗口大不等于性能好。；CoALA把记忆分为工作记忆与情节/语义/程序三类长期记忆。
- **03 核心思想**：Lost in the Middle：答案在中间时正确率可降至约35%，低于闭卷基线56.1%。；关键指令放开头用首因效应，关键约束放结尾用近因效应。；Context Rot是慢性退化：规范信号被无关内容稀释，Agent质量持续下滑。；注意力预算O(n²)：每个Token都消耗预算，最优上下文=最小Token×最高信噪比。
- **04 七类要素**：七类要素：系统指令、用户提示、状态历史、长期记忆、检索信息、可用工具、结构化输出。；系统指令落在Goldilocks Zone：给原则而非枚举，关键约束放开头。；工具定义本身是上下文：名称自描述、边界清晰、参数具体，返回值也要精简。；结构化输出要声明schema并给示例，格式稳定性可提高3-5倍。
- **05 三大核心技术**：Compaction：接近窗口上限(70%-80%)时用LLM摘要历史，重建上下文继续任务。；Note-Taking：主动把进度/决策/发现/策略笔记卸载到外部存储，实现跨会话记忆。；Sub-Agent：主Agent只收子Agent精炼摘要，用上下文隔离换规模扩展。；压缩Prompt先保召回率再保精确率；Claude Code额外保留最近5个文件。
- **06 检索策略**：检索到不相关内容比不检索更糟，只取最相关的1-2个文档。；渐进式披露：先探索（head/grep）再精准加载，避免预加载全部。；检索哲学从“预加载”转向“即时按需”（Just-in-Time）。
- **07 CE与ReAct**：四大陷阱：Observation膨胀、规则漂移、错误轨迹污染、Thought冗余积累。；规则漂移最隐蔽：系统指令被推入Lost in the Middle性能洼地。；共同本质：ReAct循环只进不出，CE专治“只进不出”。；定论：ReAct是行动框架，CE是信息保障，二者缺一不可。
- **08 代码实战**：TokenBudget：使用率≥75%触发压缩、≥60%警告，预算管理自动化。；ContextWindow映射七类要素，历史超阈值自动压缩并卸载到StructuredNotes。；JITRetriever只取最相关1-2条，信噪比优先于召回率。
- **09 误区反模式**：五大反模式：堆砌上下文、工具越多越好、系统提示过度枚举、不清理工具结果、依赖大窗口。；最优上下文不是最长而是信噪比最高；工具集“够用”而非“全面”。；大窗口只是推迟问题：Context Rot与O(n²)代价在任何窗口都存在。
- **10 Harness映射**：Harness Engineering是CE在AI Coding场景的工程化落地体系。；Rules沉淀规则、Memory/Cases积累经验、Skills标准化做法。；CE讲“管理什么”，Harness讲“怎么沉淀成团队资产”。
- **11 场景应用**：并非所有AI应用都需要完整CE体系，按场景判断。；客服用滑动窗口+外部笔记；Coding用激进压缩；研究报告用Sub-Agent。；大窗口不淘汰CE：挑战转为在百万Token中组织最优信息结构。
- **12 三句话总结**：上下文窗口是稀缺资源，无关信息主动损害信号强度。；最优上下文=最小Token数×最高信噪比。；上下文管理是持续工程工作，应自动化、可积累、可复用。

### 带走三句话

1. 上下文窗口是稀缺资源，无关信息会主动稀释关键信号，追求信噪比而非长度。
2. 长任务必须主动管理上下文：压缩、结构化笔记、子Agent隔离，别让循环只进不出。
3. 把CE原则工程化：规则沉淀、工具精简、即时检索、结构化输出，才能造出生产级Agent。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
