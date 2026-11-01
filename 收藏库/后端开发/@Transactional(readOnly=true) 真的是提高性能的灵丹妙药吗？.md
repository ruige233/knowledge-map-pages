---
title: "@Transactional(readOnly=true) 真的是提高性能的灵丹妙药吗？"
url: "https://mp.weixin.qq.com/s?__biz=MzU2OTMyMTAxNA==&mid=2247525350&idx=2&sn=3403618c41fd9e4293e44beb6100ba78"
source: "微信公众平台"
published: "2023-11-08"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发]
lang: zh
status: ok
阅读页: "阅读页/后端开发/@Transactional(readOnly=true) 真的是提高性能的灵丹妙药吗？.html"
---

# @Transactional(readOnly=true) 真的是提高性能的灵丹妙药吗？

> 一句话定位：readOnly=true 如何优化性能，又为何可能耗尽数据库连接

## 摘要

本文剖析 @Transactional(readOnly=true) 的收益与代价（JPA）。readOnly 只是提示，JpaDialect 据此把 FlushMode 置为 MANUAL 并 setDefaultReadOnly(true)，只读实体不脏检查、不维护快照、修改不持久化，还可路由只读库。实验对比：事务在存储库层查询返回即释放连接，服务层则到方法结束才释放。服务层含耗时逻辑会长期占连接，可能引发连接匮乏、死锁与吞吐下降。只读查询单独成事务时放心用，混大量其他逻辑需权衡。

## 要点

- readOnly=true 只是给事务子系统的提示，无法解释时会被静默忽略，并不强制阻止写操作。
- Hibernate 侧通过禁止 flush（FlushMode.MANUAL）并 setDefaultReadOnly(true) 实现只读优化。
- 收益：只读实体不做脏检查、不维护持久状态快照、修改不会持久化。
- 配合主从或读写副本集时，readOnly=true 可路由到只读数据库。
- 实验显示：存储库层事务查询返回即释放连接，服务层事务直到方法结束才释放。
- 服务层方法含耗时逻辑会长期占用连接，可能导致连接匮乏、死锁和吞吐下降。
- 建议：只读查询单独作为一个事务执行时放心用，混有大量其他逻辑时需权衡。

## 提炼

> 一句话定位：readOnly=true 如何优化性能，又为何可能耗尽数据库连接

### 分节提炼

- **01 工作原理**：readOnly 是提示而非强制，事务管理器可能静默忽略；JpaDialect 在 readOnly 时把 FlushMode 设为 MANUAL 禁止刷新；setDefaultReadOnly(true) 使实体不脏检查、不维护快照；只读实体的修改不会被持久化
- **02 服务层使用的取舍**：存储库层事务：查询结果一到就释放连接；服务层事务：直到方法结束才释放连接；服务层方法含耗时逻辑会长期占连接，可能连接匮乏；无限制使用事务可能导致死锁、性能与吞吐下降
- **03 回顾与结论**：readOnly=true 优点明确：性能、内存、一致性、只读库路由；服务层使用也可能死锁、性能低下、连接匮乏；只读查询单独作为事务时放心用，混大量逻辑要取舍

### 带走三句话

1. readOnly=true 是提示而非强制：靠跳过脏检查、不维护快照并路由只读库来优化。
2. 事务加在服务层会持有连接直到方法结束，含耗时逻辑时慎用以免连接饥饿。
3. 只读查询独占一个事务时放心用；方法里混大量逻辑则权衡后再加。

### 知识卡

跨全文挖掘 8 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
