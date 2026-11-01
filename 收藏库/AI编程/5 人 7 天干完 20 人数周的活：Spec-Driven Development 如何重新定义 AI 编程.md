---
title: "5 人 7 天干完 20 人数周的活：Spec-Driven Development 如何重新定义 AI 编程"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247559877&idx=1&sn=bc1f8fef9cb91d8e52aece383040d265"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI编程"
tags: [AI编程]
lang: zh
status: ok
阅读页: "阅读页/AI编程/5 人 7 天干完 20 人数周的活：Spec-Driven Development 如何重新定义 AI 编程.html"
---

# 5 人 7 天干完 20 人数周的活：Spec-Driven Development 如何重新定义 AI 编程

> 一句话定位：用 Spec 当唯一事实源，把 AI 编程从“快”变成“可控”

## 摘要

本文以“5人7天用Qoder开发QoderWork”的真实案例切入，引出Spec-Driven Development（SDD）：把Spec作为唯一事实来源，代码只是派生结果，人定义WHAT、AI实现HOW。文章拆解SDD四阶段流程（Specify/Plan/Implement/Validate）与Spec Kit三文件体系（spec.md、plan.md、tasks.md）及constitution.md项目宪法；指出好Spec需具备问题陈述、成功指标、用户故事、验收标准、非目标、约束六要素，并用“换技术栈仍有效”检验粒度。文中给出数据：API变更周期缩短75%、LLM代码错误减少50%、无Spec约束下45%代码含安全漏洞、代码重复率4年增4倍。最后对比Vibe Coding的“三个月墙”，列举五大陷阱与L1/L2/L3三级演进，主张Spec让AI可控而非变聪明。

## 要点

- 5人7天用Qoder开发出已上线产品QoderWork，完成传统20人数周的工作量，关键在DAY 0全天只写Spec。
- SDD以Spec为唯一事实来源（Single Source of Truth），人定义WHAT、AI实现HOW，代码只是派生结果。
- 标准流程四阶段：Specify→Plan→Implement→Validate，配套spec.md/plan.md/tasks.md三文件与constitution.md项目宪法。
- 好Spec六要素：Problem Statement、Success Metrics、User Stories、Acceptance Criteria、Non-Goals、Constraints，成功标准必须可测试。
- 人工精炼Spec可让LLM代码错误减少50%、API变更周期缩短75%；无Spec约束时45%的AI代码含安全漏洞。
- Vibe Coding存在“三个月墙”，SDD以Spec作为代码的压缩表示让AI读取全局约束，二者是光谱两端、可混合使用。
- 五大陷阱：过度规格化、规格腐烂、规格官僚化、虚假信心、工具复杂性；SDD不是瀑布模型，Spec是活文档。
- 演进三级：L1 Spec-First、L2 Spec-Anchored、L3 Spec-as-Source，本质价值是让AI可控而非变聪明。

## 提炼

> 一句话定位：用 Spec 当唯一事实源，把 AI 编程从“快”变成“可控”

### 分节提炼

- **01 SDD 是什么**：SDD 把 Spec 当唯一事实来源，代码是派生产物，先定义 WHAT 再让 AI 做 HOW。；AI 不会追问边界情况，Spec 质量直接决定代码质量，推断错就是 Bug。；2025 年 Vibe Coding、GitHub Spec Kit、AWS Kiro 等方向收敛，SDD 是结构性需求。；Microsoft 点评：SDD is version control for your thinking。
- **02 完整流程拆解**：四阶段 Specify/Plan/Implement/Validate，人定义 WHAT、AI 实现 HOW，流程可多轮迭代。；Spec Kit 三文件：spec.md 需求、plan.md 架构、tasks.md 任务清单，成功标准要可测试。；constitution.md 固化不可违背的团队约束，作为 AI 的“潜意识”。
- **03 Spec 怎么写**：好 Spec 六要素缺一不可，成功标准要写成 P95<200ms 这类可测硬指标。；粒度检验：换技术栈实现 Spec 仍有效，说明没把 HOW 混进 WHAT。；淘特经验：Spec 要经 3-5 次“Spec→Plan→回看 Spec”迭代才合格。
- **04 工具生态全景**：Spec Kit 通用、OpenSpec 轻量、Kiro 原生 IDE、QoderWork 的 Quest 模式贴合执行。；按通用性/轻量/生态选工具，Quest 自动写码、提 PR、跑测试。
- **05 实战数据**：金融 SDD 实践 API 变更周期缩短 75%；精炼 Spec 让 LLM 代码错误减少 50%。；Stripe 以 Harness Engineering 交付 1300 个 AI PR 且可控。；无 Spec 约束 45% AI 代码含漏洞；代码重复率 4 年增 4 倍。
- **06 SDD vs Vibe Coding**：Vibe Coding 主张忘掉代码让 AI 全权处理，原型有效但不可持续。；三个月墙：项目变大后 AI 基于局部信息决策，新功能总破坏旧功能。；SDD 的 Spec 是代码压缩表示，AI 能读全局约束；二者是光谱两端可混合。
- **07 与 Harness Engineering 关系**：三层递进：Prompt→Context→Harness Engineering，SDD 处于后两者交叉地带。；Spec 是结构化上下文；constitution/spec/plan/tasks 四层构成完整 AI 驾驭框架。
- **08 五大陷阱与局限**：五大陷阱：过度规格化、规格腐烂、规格官僚化、虚假信心、工具复杂性，各有缓解法。；SDD 非瀑布：Spec 是活文档，迭代粒度是单模块而非整个项目。
- **09 三级光谱模型**：三级光谱：L1 Spec-First 主流、L2 Spec-Anchored 测试锚定、L3 Spec-as-Source 代码由 Spec 编译。；L2 用 Acceptance Criteria 自动生成测试，CI 拒绝漂移合并；L3 人只编辑 Spec。
- **10 结语**：DAY 0 把需求边界、成功标准、约束固化为 Spec，是后六天的导航系统。；SDD 本质价值：不让 AI 变聪明，让 AI 变可控；从写一个 spec.md 开始。

### 带走三句话

1. 把“做什么”写进 spec.md，让 AI 的推断变成有锚点的执行。
2. 成功标准必须可测试：用 P95 指标替代“快”、“好用”这类形容词。
3. 从最小方案开始：一个 spec.md 加一个 AI Agent，先小步跑起来。

### 知识卡

跨全文挖掘 11 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
