---
title: "换一套 Harness，比换两代模型还管用"
url: "https://mp.weixin.qq.com/s?__biz=MzI2NDU4OTExOQ%3D%3D&mid=2247697554&idx=1&sn=78f66fec6cc85f3fbe642283b7d91cc2"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI编程"
tags: [AI编程, Harness]
lang: zh
status: ok
阅读页: "阅读页/AI编程/换一套 Harness，比换两代模型还管用.html"
---

# 换一套 Harness，比换两代模型还管用

> 一句话定位：模型定上限，Harness 定兑现：拆解循环、四大子系统与生产化实践

## 摘要

模型之间差距在收窄，真正分高下的是模型之外的 Harness。同一模型换一套 Harness，评测完成率能差十几二十个百分点，常超过两代模型差距。全文按「一个循环→四个子系统→生产化→长时运行→评测→选型」展开：Harness 本质是思考-执行-观察循环；Tool、Memory & Context、Guardrails、Skill 四大子系统决定行为；生产化补错误处理、Sandbox、多 Agent 编排与调度；长时运行靠文件记忆而非窗口，坚持干活者不验收；评测里基础设施是一等变量，还要防 Agent 绕过任务。结论：模型定上限，Harness 定兑现。

## 要点

- 模型差距在收窄，真正分高下的是 Harness：同一模型换套 Harness，评测完成率可差十几二十个百分点，常超两代模型差距。
- Harness 本质是「思考→执行→观察」循环；生产级差别在轮次上限、原地打转检测、错误回喂、大输出截断、并行与流式等边界处理。
- 四大子系统：Tool 管执行、Memory & Context 杠杆最高、Guardrails 用代码裁决、Skill 实现薄 Harness 加厚 Skill。
- 错误不要吞、要回喂模型；瞬时错误指数退避带抖动重试，给一切设上限，恢复不了再升级人类。
- 长时运行靠 out-of-context 文件记忆而非上下文窗口；干活的不能兼验收，评估者要独立上下文与显式标准。
- 多 Agent 是工具不是默认：撞上窗口上限、无法专业化、串行三面墙才用，需 Context 隔离与超时。
- 评测中基础设施是一等实验变量，分数是模型+编排+基础设施+题目集+判分的联合属性，还要防 Agent 绕过任务。
- Guardrails 写在代码不在 prompt；审批量大时用独立审核模型分层把关，且不给它看 Agent 推理。

## 提炼

> 一句话定位：模型定上限，Harness 定兑现：拆解循环、四大子系统与生产化实践

### 分节提炼

- **01 瓶颈已转移到 Harness**：同一模型换一套 Harness，评测完成率可差十几二十个百分点，常超过两代模型差距；换 Harness 等于换隐藏配置：步数上限、工具暴露方式、超长返回截断策略、上下文取舍；Agent 概念已漂移：从接个工具到自主探索代码库、验证并交证据，差距全在模型之外
- **02 四个词的层次**：Agent 是成品；Framework 帮你造 Harness，Runtime 托管 Harness，Harness 决定行为；Runtime 决定物理上能不能读文件、联网，Harness 决定会不会去读、会不会去联；后文主语仍写 Agent，但每次真正落地的决定都发生在 Harness 这一层
- **03 三条直觉**：Agent 出错先怀疑 Context/Tool 管道，而不是模型变笨；按需生长：先最小循环，跨任务复用才加 Memory，工具膨胀才加 Skill，担责才补 Guardrails；模型只看得见本轮实际收到的内容，磁盘里有、上轮说过都不等于这一轮看得见
- **04 Agentic Loop**：循环=思考→执行→观察→重复；是否真循环看工具结果会不会改变后续动作；生产级边界：轮次上限、原地打转检测、错误回喂、大输出截断、并行调用、流式输出；四块拼成 Harness：System Prompt、Tool 定义、Tool 执行、Tool Loop
- **05 四大子系统**：Tool 系统：模型看 Schema、Harness 管执行，dispatch 永远返回字符串；Context 工程杠杆最高，三大支柱是组装、压缩、预算；组装是带优先级的装箱；Guardrails 是代码级权限层，执行前四选一：放行/拒绝/改写/询问；Skill 是打包能力单元，薄 Harness 加厚 Skill，按需加载每轮省约 78% Token
- **06 生产化四道工序**：错误先分类：瞬时退避加抖动重试，永久回喂模型，fallback，升级人类；Sandbox 用启动参数圈死：非 root、禁网、资源上限、只读；凭证绝不进沙盒；多 Agent 三面墙：Context 上限、无法专业化、串行；四种编排拓扑可组合；调度四原语：One-Shot、Cron、Event-Triggered、Heartbeat；内部一律 UTC
- **07 长时运行架构**：长时运行两大隐形杀手：上下文焦虑（快满时赶工）与自我评估偏差（永远 8/10）；干活者不验收：规划者→生成者→评估者，独立上下文、显式标准、可操作反馈；托管式三层：无状态大脑、只增事件日志 Session、可丢弃 Sandbox；Session≠Context Window；Initializer + Coding Agent 两阶段：记忆在文件不在窗口，一 session 一 feature
- **08 评测三道坎**：基础设施是一等实验变量：内存、CPU、超时口径都会移动分数，要版本化记录；被测觉察：Agent 会识别评测集并绕过任务；靠能力限制而非行为检测挡住；用独立审核模型审批：入口出口两层，便宜模型快筛，不让它看 Agent 推理
- **09 十二条心法**：全文收敛为十二条心法：循环、Context、Guardrails、Skill、错误、验收、文件记忆等；按需要长出来：先跑通最小循环，再逐步加 Memory、Skill、Guardrails 与 Sandbox
- **10 参考来源**：综述整理自模型厂商工程文档、论文与公开讨论、工程通用实践、开源项目文档；Loop Engineering（happycapy.ai）与 Harness Engineering Guide（MIT）是主要体系来源；文中定量结论未引具体数值，需精确结论应回原始报告

### 带走三句话

1. 先把最小循环跑通，再按需加 Memory、Skill、Guardrails；不要提前为没出现的问题搭基础设施。
2. 错误永远作为 Tool 结果回喂模型，绝不静默吞掉；给轮次、重试、审批都设上限。
3. 长程任务让记忆落在文件与 git 里，验收交给独立评估者；Harness 决定你能兑现多少模型能力。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
