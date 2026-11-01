---
title: "面试官：会SQL调优，那你知道索引合并吗"
url: "https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247538364&idx=1&sn=18d3231f98be1ed090d7822541993240"
source: "微信公众平台"
published: "2023-09-13"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发, 面试]
lang: zh
status: ok
阅读页: "阅读页/后端开发/面试官：会SQL调优，那你知道索引合并吗.html"
---

# 面试官：会SQL调优，那你知道索引合并吗

> 一句话定位：MySQL 索引合并：explain 识别 index_merge 与三种算法

## 摘要

本文讲解 MySQL 索引合并（Index Merge）：explain 的 type=index_merge 表示同时使用多个索引，先查主键并合并再一次回表。Index Merge 只合并单表多次 range 扫描，分三种算法：交集（intersect）用于 AND 条件；并集（union）用于不同索引列 OR 关联；排序并集（sort_union）用于 OR 范围查询，先排序再回表。算法受 optimizer_switch 控制；复杂 AND/OR 嵌套可用恒等变换改写；不适用于全文索引。

## 要点

- explain 的 type 列为 index_merge 时表示使用了索引合并优化，key 列列出具体使用的索引。
- 索引合并先按各索引查到主键并合并，再根据合并后的主键一次回表取结果。
- Index Merge 仅合并单表多次 range 扫描的结果，可产生并集、交集或交集并集，对应三种算法。
- 交集算法用于 AND 关联的多条件查询，要求条件覆盖索引的全部部分，并对主键取交集。
- 并集算法用于不同索引列用 OR 关联或 InnoDB 主键范围查询，对结果取并集。
- 排序并集算法用于 OR 组合的多个范围查询，需先获取行 ID 并排序再回表。
- 三种算法显示在 Extra 字段，受 optimizer_switch 的 index_merge_* 标志控制，默认全部开启。
- 复杂 AND/OR 嵌套可用恒等变换改写帮助优化器选计划；Index Merge 不适用于全文索引。

## 提炼

> 一句话定位：MySQL 索引合并：explain 识别 index_merge 与三种算法

### 分节提炼

- **01 算法总览与限制**：Index Merge 不适用于全文索引（FULLTEXT）。；Extra 字段展示具体算法：intersect / union / sort_union。；三种算法受 optimizer_switch 的 index_merge_* 标志控制，默认全部开启。；优化器根据成本估算在三种算法间选择。
- **02 交集算法**：多条 AND 条件分别命中不同索引时可走 intersect，对主键取交集。；需满足 key_part1=const1 AND … AND key_partN=constN 等条件之一。；InnoDB 主键范围条件不用于行检索，只用于过滤其他条件检出的行。；若查询列全被索引覆盖，则不回表取完整表行。
- **03 并集算法**：不同索引列用 OR 关联时走 union，对结果取并集。；适用条件之一：key_part1=const1 OR … OR key_partN=constN。；InnoDB 主键范围查询也可触发 union。；示例：merchant_id=3 OR area=3。
- **04 排序并集算法**：OR 组合的多个范围查询走 sort_union。；先获取所有行的行 ID 并排序，再回表。；与 union 的区别在于增加了排序步骤。；示例：merchant_id<3 OR area<3。

### 带走三句话

1. explain 看到 type=index_merge，说明走了多索引合并，key 列可确认具体索引。
2. AND 走 intersect、OR 等值或主键范围走 union、OR 范围走 sort_union，后者先排序再回表。
3. 算法开关在 optimizer_switch，默认全开；全文索引不适用 Index Merge。

### 知识卡

跨全文挖掘 9 张知识卡（代码模式 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
