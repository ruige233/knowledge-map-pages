---
title: "「纯干货」几万字都讲不明白的Memory架构与思考"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247559363&idx=1&sn=461e525a0373ccfed0a7638cc1c51906"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "其他"
tags: [其他]
lang: zh
status: ok
阅读页: "阅读页/其他/「纯干货」几万字都讲不明白的Memory架构与思考.html"
---

# 「纯干货」几万字都讲不明白的Memory架构与思考

> 一句话定位：提出 Agent 记忆三件套抽象，并论证非参数化逼近参数化上限

## 摘要

本文提出 Agent Memory 的系统化抽象：记忆本质是 event 序列 / Raw Ledger + Views + Policy 三件套，而非“向量库+prompt”。核心命题：记忆是可被决策利用的外部状态；最小闭包是(Ledger,Views,Policy)；基本单位是 event 序列。记忆能力与通用 Agent 能力相对正交，需外置 System 2，以非参数化方案逼近参数化上限；上限由接口带宽、检索聚合误差、policy 可学习性三类瓶颈决定。

## 要点

- 记忆的本质不是“存储”，而是把历史转成当前可用信息、可影响决策分布的外部状态。
- Memory 的最小闭包是 Raw Ledger（账本）+ Views（派生视图）+ Policy（控制层）三件套，缺一不可。
- 记忆的基本单位是 event 序列，但 event 只提供可审计历史，能力来自 views 与 policy。
- 记忆能力与通用 Agent 能力相对正交，因此需要显式外置的 System 2 承担读写与检索。
- 非参数化记忆的上限由接口带宽、检索聚合误差、policy 可学习性与可控性三类瓶颈决定。
- 时序是 Memory OS 的结构维度：bi-temporal + time-sliced recall 区分“曾经为真”与“当前为真”。
- 过程记忆的关键是“可执行+可验证+可治理”，把成功轨迹固化为可复用 skill。
- 最终收敛为内核、文件系统、可执行文件、总线接口、学习引擎五件套抽象。

## 提炼

> 一句话定位：提出 Agent 记忆三件套抽象，并论证非参数化逼近参数化上限

### 分节提炼

- **01 全文**：Memory 最小闭包 = Ledger+Views+Policy，分别负责溯源、可用、可控迭代；记忆能力与通用 Agent 能力相对正交，应外置 System 2 换取可插拔、可迁移、可归因；非参数化上限看三类瓶颈：接口带宽、检索聚合误差、policy 可学习性；时序是结构维度：bi-temporal + time-sliced recall 区分历史与当前事实

### 带走三句话

1. 记忆价值不在“存了多少”，而在“历史到当前决策”的通道是否有效——账本、视图、策略缺一不可。
2. 把时序当结构维度落地：bi-temporal + time-sliced recall 让“曾经为真”与“当前为真”分开。
3. 非参数化逼近参数化的瓶颈在 policy 与注入带宽，控制回路要显式可训练、可回放、可 A/B。

### 知识卡

跨全文挖掘 10 张知识卡（数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
