---
title: "让 AI 自己做增长：基于OPC和Harness思想的自主增长系统探索"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247560242&idx=1&sn=37875b95003fe7c3c95e2966e48878f7"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI编程"
tags: [AI编程, Harness]
lang: zh
status: ok
阅读页: "阅读页/AI编程/让 AI 自己做增长：基于OPC和Harness思想的自主增长系统探索.html"
---

# 让 AI 自己做增长：基于OPC和Harness思想的自主增长系统探索

> 一句话定位：基于Harness的多Agent自主增长系统：AI自主提案、开发、评审到上线。

## 摘要

作者团队围绕高德PC站SEO增长，借鉴OPC（一人公司）思路，基于Harness Engineering构建AI自主增长系统：由总控、记忆、专业Agent与skill分工，让AI自主提案、编码、评审、部署。以状态机workflow管理长任务，配心跳、超时恢复与失败处理；拆分Planner/Builder/Evaluator防上下文污染，以评审分离、零信任、零Broken Feature保评估独立。用Benchmark做“元评估”量化评审能力，三轮把均分从64.5提到83.4。

## 要点

- 借鉴OPC（一人公司）思路，让AI在高德PC站SEO场景自主发现机会、提案、开发、测试、上线。
- 基于Harness Engineering构建系统：总控Agent＋记忆系统＋拆分专业Agent＋垂类skill。
- 用状态机workflow解决长任务：心跳监控、状态流转、超时恢复、进入条件、产出校验、失败处理。
- 拆分Planner/Builder/Evaluator并每次新建SubAgent，避免上下文污染与爆炸，保证评估独立。
- 三条评审原则：评审与生成彻底分离、零信任、零Broken Feature。
- 用Benchmark做“元评估”量化评审能力，三轮优化均分64.5→83.4、精确匹配率25%→78%。
- 工程化踩坑：环境工具化、评审报告标准化、快速失败优先、端到端串联难度被低估。
- “完全无人干预”非0/1：把人工干预降到足够低，线上部署仍保留人工确认，人从执行者变监护者。

## 提炼

> 一句话定位：基于Harness的多Agent自主增长系统：AI自主提案、开发、评审到上线。

### 分节提炼

- **01 背景与动机**：OPC（一人公司）下AI Agent可独立完成发现机会到设计、编码、测试、上线的全链路。；用“路书”功能验证：PRD编写到发布日常环境全程0人为介入，连续运行4小时无P0 Bug。；传统做法每个环节需专人参与，或人工指挥多个AI工具协作。
- **02 系统设计思路**：Harness Engineering围绕Coding Agent构建约束、反馈回路与持续改进循环。；Anthropic实践：Planner扩规格、Generator做前端、Evaluator实测回传。；业务架构需总控Agent、记忆系统共享知识、专业Agent拆分与垂类skill。
- **03 多Agent落地**：状态机workflow＋心跳监控＋超时恢复＋失败处理＋评审门禁保障长任务运行。；Planner拆3、Builder拆2、Evaluator拆6代理，每次新建SubAgent防污染。；评审三原则：评审与生成彻底分离、零信任、零Broken Feature。；Benchmark做元评估：评评审能力而非代码质量，三轮均分64.5→83.4。
- **04 踩坑与解决**：能工具化的底层工具都要面向AI工具化，本地与云端环境统一保障稳定运行。；评审报告按CRITICAL/MAJOR/MINOR分级，识别阻塞问题即时反馈builder。；快速失败：环境变量检查1秒拦住，避免近4分钟无效评审。；线上部署仍保留人工确认；完全无人干预＝低干预＋一人监护多任务。
- **05 未来思考**：系统仍属阶段性探索，质量未达高标准，需持续优化系统工程化。；元评估受数据集覆盖度制约，需自建Product/Design/Arch/Testcase数据集。；OPC＋Agent在特定领域已可用，上限随AI能力提升而持续放大。

### 带走三句话

1. Harness Engineering不是让AI回答一次，而是用状态机、门禁、反馈回路保证长任务稳定可交付。
2. 评估独立靠拆分实现：评审与生成分离、零信任、零Broken Feature，并用Benchmark量化评审能力。
3. 工程化比模型更难：环境工具化、快速失败、可追溯日志、保留人工上线卡点，让人从执行者变监护者。

### 知识卡

跨全文挖掘 13 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
