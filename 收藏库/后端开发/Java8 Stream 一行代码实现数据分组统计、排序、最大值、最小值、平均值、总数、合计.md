---
title: "Java8 Stream 一行代码实现数据分组统计、排序、最大值、最小值、平均值、总数、合计"
url: "https://mp.weixin.qq.com/s?__biz=MzU4MDUyMDQyNQ==&mid=2247519207&idx=1&sn=fb725eb337b04844efac078987cacf27"
source: "微信公众平台"
published: "2023-09-28"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发, Java]
lang: zh
status: ok
阅读页: "阅读页/后端开发/Java8 Stream 一行代码实现数据分组统计、排序、最大值、最小值、平均值、总数、合计.html"
---

# Java8 Stream 一行代码实现数据分组统计、排序、最大值、最小值、平均值、总数、合计

> 一句话定位：Java8 Stream 分组统计、排序、求和等聚合操作示例合集

## 摘要

本文演示 Java8 Stream 对集合的分组统计、排序与聚合写法。核心是 Collectors：groupingBy 按字段分组计数，summarizingInt 一次取得每组的最大、最小、求和、平均值与总数。另有 min/max 求最值、mapToInt().sum() 求和、averagingInt 求平均、joining 拼接并自定义前后缀与分隔符。还演示按部门分组、双重分组计数、collectingAndThen 配合 maxBy 取每组年龄最大的人。

## 要点

- Java8 Stream 不改变原数据即可完成分组统计、排序、求和等处理。
- summarizingInt 配合 groupingBy 可一次性得到每组 status 的最大、最小、求和、平均值与总数。
- groupingBy 可返回 Map，key 为分组字段、value 为计数或元素列表。
- min/max 搭配 Comparator.comparing 求最值，返回 Optional 需判断 isPresent。
- mapToInt(...).sum() 求和，averagingInt 或 mapToDouble(...).average() 求平均。
- Collectors.joining 可拼接字段并自定义前缀、后缀和分隔符。
- groupingBy 支持双重分组与 collectingAndThen(maxBy) 取每组年龄最大的人。

## 提炼

> 一句话定位：Java8 Stream 分组统计、排序、求和等聚合操作示例合集

### 分节提炼

- **01 全文**：Stream 链式 collect 配合 Collectors 可完成分组、计数、汇总统计。；summarizingInt 返回 IntSummaryStatistics，一次取出 count/min/sum/average。；min/max 返回 Optional，需先 isPresent 判断。；groupingBy 支持嵌套分组与 collectingAndThen 取每组最值。

### 带走三句话

1. 分组统计优先用 groupingBy，需要一次多指标时叠加 summarizingInt。
2. 求最值用 min/max + Comparator.comparing，记得处理 Optional。
3. 双重分组、拼接、取组内最大都能用 Collectors 一行完成。

### 知识卡

跨全文挖掘 8 张知识卡（代码模式 / 易错点 / 概念对比），带原文锚点，完整内容见阅读页提炼层。
