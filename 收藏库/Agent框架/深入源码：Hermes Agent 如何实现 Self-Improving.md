---
title: "深入源码：Hermes Agent 如何实现 Self-Improving"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247559661&idx=1&sn=ca9426f948819f172ec44f671127aa29"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent框架, Agent]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/深入源码：Hermes Agent 如何实现 Self-Improving.html"
---

# 深入源码：Hermes Agent 如何实现 Self-Improving

> 一句话定位：Memory+Skill+Nudge：Hermes 自进化闭环三子系统

## 摘要

Hermes Agent 通过 Memory、Skill、Nudge Engine 三子系统实现自我进化。Memory 用两个纯文本文件记事实与偏好，2200/1375 字符上限加冻结快照，超限由模型自行删改；Skill 在复杂任务成功后自动创建，踩坑后模糊匹配局部 patch，按需加载；Nudge Engine 用回合/迭代双计数器后台 fork 审查，静默反思。K8s 案例中调用 12→6 次、错误归零；安全靠威胁扫描与安全扫描回滚。RDSHermes 产品化后补齐数据库纳管与凭证托管。

## 要点

- Hermes 靠 Memory、Skill、Nudge Engine 三个子系统构成 Self-Improving 闭环，越用越强。
- Memory 限 2200/1375 字符并冻结快照，超限时让模型自行删改，倒逼信息压缩。
- Skill 由 Agent 在复杂任务成功后自动创建，踩坑后 patch 修补，渐进式加载省 Token。
- Nudge Engine 用回合/迭代双计数器触发，后台 fork 审查 Agent，静默反思不打扰用户。
- 完整案例中 K8s 部署工具调用从 12 次降到 6 次、错误归零，体现经验复利。
- 安全机制两层防护：Memory 威胁扫描与 Skill 安全扫描，不通过即回滚。
- RDSHermes 将自进化能力产品化，补齐数据库安全纳管、凭证托管与全链路审计。
- OpenClaw 手写 Skill 无法自主学习，架构缺创建触发、patch 机制与 review agent。

## 提炼

> 一句话定位：Memory+Skill+Nudge：Hermes 自进化闭环三子系统

### 分节提炼

- **01 背景**：Hermes 增速 +204%，GitHub 106k+ Star；核心差异：Agent 自动提炼 Skill，OpenClaw 靠人喂
- **02 三子系统总览**：Memory 记事实、Skill 记步骤、Nudge Engine 定时提醒复盘；三个子系统构成学习闭环
- **03 Memory 系统**：MEMORY.md/USER.md 限 2200/1375 字符，倒逼压缩；超限时 add 失败并返回条目，让模型主动删改；冻结快照共享前缀缓存，会话内不变；写声明式事实而非命令式指令
- **04 Skill 系统**：SKILL.md 是核心，含 Steps/Pitfalls；创建门槛：5+ 次调用、踩坑修复、用户纠正；patch 局部修补 + 安全扫描回滚；先看目录再按需 skill_view 加载
- **05 Nudge Engine**：Memory 按回合、Skill 按迭代，双计数器默认 10；后台 fork 审查 Agent，输出静默不打扰；禁用审查实例自身 nudge，避免无限递归
- **06 三次会话案例**：第 1 次 12 次调用踩两坑后创建 Skill；第 2 次复用降到 9 次并 patch ALLOWED_HOSTS；第 3 次 6 次调用零错误
- **07 安全机制**：Memory 威胁模式扫描防注入与外泄；Skill 安全扫描不通过即回滚；团队落地需凭证加密托管，密钥不落盘
- **08 设计取舍**：字符上限、冻结快照、后台审查等均有成本考量；Memory/Skill 进系统提示词，是一等安全边界
- **09 进化方向**：frontmatter 加使用统计做生命周期管理；识别常组合 Skill 合成工作流；创建后通知用户审核，团队治理可追溯
- **10 RDSHermes 落地**：把自进化能力包装成开箱即用服务；补齐数据库安全纳管、凭证托管、审计监控；DBA 巡检从 40 分钟缩短到 2 分钟
- **11 总结**：三件事配合：Memory 记你是谁、Skill 记怎么做、Nudge 保循环；v0.6.0 补多实例、MCP、迁移工具，切换门槛降低

### 带走三句话

1. Agent 干完活把踩坑经验提炼成 Skill，下次同类任务直接调用，越用越强。
2. Memory 存事实、Skill 存步骤、Nudge 定时复盘，三者闭环是自进化最小组合。
3. 自进化必须配安全约束：威胁扫描、安全扫描与回滚，否则记忆可被劫持。

### 知识卡

跨全文挖掘 11 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
