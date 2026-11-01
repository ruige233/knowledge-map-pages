---
title: "Agent核心技术概念与范式发生了哪些演变以及背后的思考"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA%3D%3D&mid=2247560220&idx=1&sn=72a83c5317c93a91f78daf6b203d308d"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent框架, Agent]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/Agent核心技术概念与范式发生了哪些演变以及背后的思考.html"
---

# Agent核心技术概念与范式发生了哪些演变以及背后的思考

> 一句话定位：复盘 Agent 四阶段演进与六大核心概念的底层逻辑变化

## 摘要

本文复盘 2023–2026 年 Agent 范式演化：被动 ReAct、Workflow、自主 Agent、自进化四阶段并存互补。六大概念均发生演变：Prompt 深耦合转向渐进式加载，Planning 思维链转向复杂长程任务，Memory 检索增强转向文件系统化，Tools 由 Function Call 转向 CLI 与脚本，Workflow 刚性编排转向 Skill 封装，Environment 无状态转向隔离运行时。内核从“魔法调优”走向“系统工程”，以工程化确定性承载模型不确定性。

## 要点

- Agent 经历被动 ReAct、Workflow、自主 Agent、自进化四个阶段，并存互补而非替代
- Prompt 从深耦合单体 Prompt 转向固化 System Prompt + 渐进式加载，实现动静分离
- Planning 从线性思维链升级为结构化分解、长程推理与子 Agent 动态构建
- Memory 从 RAG 向量检索转向文件系统化 + 向量检索混合管理
- Tools 从 Function Call 转向 CLI 原生化与 Script 脚本化
- Workflow 从刚性编排转向 Skill 封装与混合架构；Environment 从无状态转向隔离运行时
- 内核重构：从魔法调优走向系统工程，以工程化确定性承载模型不确定性

## 提炼

> 一句话定位：复盘 Agent 四阶段演进与六大核心概念的底层逻辑变化

### 分节提炼

- **01 背景与写作动机**：新旧技术概念混杂易混淆，营销号转载早期文章加剧困惑；演化前后并非简单替代，技术可相互结合使用；写作目的是对比演化前后范式，帮助理清思路与技术选型
- **02 四阶段演进**：四阶段：被动 ReAct、Workflow、自主、自进化，并存互补；早期被动式响应依赖明确指令，难以胜任长链任务；Workflow 用工程化约束补模型不确定性，to B 落地最稳定；自主与自进化阶段具备长程规划、自我校验与经验沉淀
- **03 六大概念演进**：Prompt 深耦合转向固化 System Prompt + 渐进式加载，动静分离；Planning 从线性思维链升级为结构化分解与长程推理；Memory 由 RAG 转向文件系统化 + 向量检索混合管理；Tools转CLI/Script；Workflow转Skill；Environment转隔离运行时
- **04 总结**：宏观架构仍是经典模块，形未变但神已变；从魔法调优转向系统工程，用工程化手段放大模型优势；以工程化确定性承载模型不确定性，是长期基石

### 带走三句话

1. 四个阶段并非替代而是并存互补，选型看复杂度、稳定性与成本预算
2. 六大概念的共同主线是用文件化、CLI 原生、脚本化与沙箱隔离放大模型优势
3. 理解演进逻辑比掌握具体工具更重要，工程化确定性是长期基石

### 知识卡

跨全文挖掘 11 张知识卡（数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
