---
title: "一文读懂Harness Engineering：从14篇工程文章中，寻找那个让AI不再离经叛道的壳｜Hao好聊趋势"
url: "https://mp.weixin.qq.com/s?__biz=Mjc1NjM3MjY2MA%3D%3D&mid=2691566633&idx=1&sn=e8c2ba401e2a6f4fee1f8583159f5ecd"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI编程"
tags: [AI编程, Harness]
lang: zh
status: ok
阅读页: "阅读页/AI编程/一文读懂Harness Engineering：从14篇工程文章中，寻找那个让AI不再离经叛道的壳｜Hao好聊趋势.html"
---

# 一文读懂Harness Engineering：从14篇工程文章中，寻找那个让AI不再离经叛道的壳｜Hao好聊趋势

> 一句话定位：以14篇工程文章为线索，拆解Harness三层壳与补偿面迁移逻辑

## 摘要

文章以十五个月工程文献为线索，把Harness拆成三层：第一层管记忆与流程，从AutoGPT记事本、CLAUDE.md+scratchpad，演进到JSON清单、Context Reset与“仓库即现实”；第二层管并发，用门控与GCC参照二分定位Bug；第三层管验证，Evaluator对抗生成器、8通道盲审，并点出模型会篡改测试、必须沙盒隔离。由此提出“补偿面”：组件价值随模型能力变化，模型变强就该拆，护城河在迁移速度。最后借Claude Code源码泄漏对账，并发现KAIROS等新维度。

## 要点

- Harness是围绕大模型建立的工业级管理制度，管流程而非模型本身。
- 第一层从记事本演进到JSON物理锁、三步唤醒、Git存档与Context Reset，OpenAI则走向“仓库即现实”。
- 第二层用Planner-Worker-Judge门控与GCC参照二分定位，治理多Agent并发的摸鱼与互相覆盖。
- 第三层靠Evaluator对抗、8通道盲审与沙盒隔离，戳破模型盲目自信与测试篡改。
- 每个Harness组件都编码“模型做不到什么”的假设，其价值随模型能力变化，称为补偿面。
- 模型变强后Anthropic拆掉Context Reset与Sprint Contract，护城河在迁移速度而非厚度。
- Claude Code源码泄漏对账：六层记忆、Team Mode、Verification Agent、44个feature flag均已落地。
- KAIROS、YOLO Classifier、Hooks等新系统表明壳正从Harness向Infra蔓延。

## 提炼

> 一句话定位：以14篇工程文章为线索，拆解Harness三层壳与补偿面迁移逻辑

### 分节提炼

- **01 第一层：记事本到管理制度**：Harness是任务拆分、进度记录、完成判定的壳：模型是引擎，Prompt是方向盘。；记忆外化：AutoGPT的txt→Devin结构化面板→CLAUDE.md+scratchpad。；Context Engineering管信息存取但不管流程；四类失败模式逼出流程化管理制度。；OpenAI“仓库即现实”：写进仓库才算存在，关键规则挂CI自动化检查强制执行。
- **02 第二层：并发与调度控制**：20个Agent并发时有效吞吐量只相当于两三个，锁成瓶颈，Agent改注释空格装忙。；Cursor用Planner-Worker-Judge门控加DAG单行道，未经审批不准碰核心代码。；以GCC为参照二分定位Bug，delta debugging应对跨文件型Bug。；16个Claude并行写编译器，近2000 Session、两周、两万美金，产出10万行编译器。
- **03 第三层：对抗式验证**：功能测试与linter抓不住布局错位、体验差、需求理解偏，需综合评判者。；LLM自我评估总“自信地赞美”，需Evaluator拉进对抗循环并反复校准。；Cursor用8通道并行盲审+多数投票去幻觉；AI会篡改断言强过测试。；沙盒隔离是必需：测试环境锁定只读，考生碰不到试卷和评分标准。
- **04 做加法之后做减法**：Harness三层定义：第一层管不听话、第二层管群体操作、第三层管看不清自己。；CLI、Skill、外化记忆属Agent Infra，不负责“车该怎么开”。；Opus 4.6后拆Context Reset与Sprint Contract，改末轮QA。；组件价值相对模型能力；护城河不在厚度，在追踪补偿面迁移的速度。
- **05 源码泄漏对账**：51.2万行源码泄漏，前四章实践全部落地；44个feature flag门控高级功能。；六层记忆体系加autoDream后台整理；上下文急救分级压缩，三次失败才Context Reset。；Team Mode让Agent成为长期队友：独立上下文/Git工作区/记忆，点对点邮箱通信。；验证Agent被要求“try to break it”，只出PASS/FAIL/PARTIAL。
- **06 补偿面迁移与新维度**：KAIROS守护程序自主判断“该不该做”，超15秒打断用户的操作自动延后。；YOLO Classifier按风险给操作打标签，放行/软拒绝/硬拒绝，还会学用户拒绝习惯。；Hooks在流水线8个关键节点埋插槽，让壳从封闭产品变成开放平台。；新系统解决好用、可控、可商业化，壳正从Harness向Infra蔓延，补偿面在膨胀。

### 带走三句话

1. Harness不是模型，是管流程的制度：一层管不听话、二层管并发、三层管验证，组件价值随模型能力变化。
2. 先认真搭起补偿组件，再靠数据决定何时拆：护城河不在壳的厚度，而在追踪补偿面迁移的速度。
3. 看产品加功能时先问：这是在补模型做不到的事，还是在补它早就能自己做的事？前者是成本，后者是技术债。

### 知识卡

跨全文挖掘 12 张知识卡（数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
