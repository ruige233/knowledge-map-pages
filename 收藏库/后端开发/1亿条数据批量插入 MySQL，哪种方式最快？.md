---
title: "1亿条数据批量插入 MySQL，哪种方式最快？"
url: "https://mp.weixin.qq.com/s?__biz=MzIwNjg4MzY4NA==&mid=2247526137&idx=2&sn=63f9600aafcb892f2c676f4ddd1921f7"
source: "微信公众平台"
published: "2023-09-24"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发, MySQL]
lang: zh
status: ok
阅读页: "阅读页/后端开发/1亿条数据批量插入 MySQL，哪种方式最快？.html"
---

# 1亿条数据批量插入 MySQL，哪种方式最快？

> 一句话定位：亿级数据插入 MySQL 最快方案：JDBC 批处理+事务混合

## 摘要

为获得足够量级的测评数据，作者随机生成姓名、年龄、性别、电话、Email、住址等人员数据，对比三种策略、五种情况向 MySQL 大批量插入数据的效率：Mybatis 轻量级框架插入（无事务）、JDBC 直接处理（开/关事务）、JDBC 批处理（开/关事务）。结果显示 Mybatis 无事务插入一万条约 28.6 秒，五百万条测试因太慢中途终止；JDBC 直接处理无事务约 21.2 秒/万条、开事务约 3.9 秒/万条；JDBC 批处理无事务约 2.1 秒/十万条、开事务约 1.9 秒/十万条。结论是：大量单条数据插入时，使用 JDBC 批处理与事务混合速度最快，实测 1 亿条耗时 174756 毫秒。批处理需在 URL 开启 rewriteBatchedStatements=true 且 useServerPrepStmts=false，并将 PreparedStatement 预处理放在循环体外。

## 要点

- 用 EXPLAIN 比较 SQL 时 10 万级测试数据太少难以下结论，故生成大量随机人员数据做插入效率测评
- 三种策略五种情况：Mybatis 轻量级框架插入（无事务）、JDBC 直接处理（开/关事务）、JDBC 批处理（开/关事务）
- 效率排序：Mybatis 轻量级插入 < JDBC 直接处理 < JDBC 批处理
- Mybatis 无事务插入一万条耗时约 28.6 秒，五百万条测试因太慢中途终止
- JDBC 直接处理无事务约 21.2 秒/万条、开事务约 3.9 秒/万条，差距约 5 倍且随数据量增大而增大
- JDBC 批处理无事务约 2.1 秒/十万条、开事务约 1.9 秒/十万条
- 大量单条数据插入使用 JDBC 批处理+事务混合最快，实测 1 亿条耗时 174756 毫秒
- 批处理需 URL 开 rewriteBatchedStatements=true、useServerPrepStmts=false，PreparedStatement 预处理放循环体外

## 提炼

> 一句话定位：亿级数据插入 MySQL 最快方案：JDBC 批处理+事务混合

### 分节提炼

- **01 JDBC批处理前置配置**：URL 需携带 rewriteBatchedStatements=true、useServerPrepStmts=false 开启批处理与预编译；PreparedStatement 预处理 SQL 必须放在循环体外；循环内 addBatch 累积，循环外 executeBatch 统一执行
- **02 总结与结论**：开启事务下 JDBC 直接处理与 JDBC 批处理耗时均更短；JDBC 直接处理开/关事务差距约 5 倍，且随数据量增大而增大；JDBC 批处理开/关事务差距微小，但开启事务仍有提升；大量单条数据插入用 JDBC 批处理+事务混合最快，1 亿条耗时 174756 毫秒
- **03 千万级补充测试**：补充测评：JDBC 批处理事务开/关各插 20 次、每次 50W，共 1000 万条；开/关事务的完整结果见下方两小节，总体差距不大
- **04 补充：开启事务**：开启事务插入 1000W 数据共耗时 197654 毫秒；这是批处理+事务开启的千万级补充结果
- **05 补充：关闭事务**：关闭事务插入 1000W 数据共耗时 200540 毫秒；与开启事务相差约 2886 毫秒，差距不大

### 带走三句话

1. 批量插入 MySQL 优先用 JDBC 批处理+事务，实测 1 亿条仅 174756 毫秒
2. JDBC 直接处理开事务比关事务快约 5 倍，批处理下二者差距很小
3. 批处理记得 URL 开 rewriteBatchedStatements=true，并把预处理 SQL 放循环体外

### 知识卡

跨全文挖掘 9 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
