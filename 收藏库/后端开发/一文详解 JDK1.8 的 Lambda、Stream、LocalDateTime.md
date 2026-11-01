---
title: "一文详解 JDK1.8 的 Lambda、Stream、LocalDateTime"
url: "https://mp.weixin.qq.com/s?__biz=MzAwMTk4NjM1MA==&mid=2247517358&idx=1&sn=fc0b60eee253cbddeec1c5bd35a44a85"
source: "微信公众平台"
published: "2023-09-28"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发]
lang: zh
status: ok
阅读页: "阅读页/后端开发/一文详解 JDK1.8 的 Lambda、Stream、LocalDateTime.html"
---

# 一文详解 JDK1.8 的 Lambda、Stream、LocalDateTime

> 一句话定位：JDK1.8 Lambda、Stream、LocalDateTime 用法速查

## 摘要

本文梳理 JDK 1.8 三大新特性。Lambda 部分讲匿名函数、参数与花括号省略规则，并用遍历和线程创建对比传统写法，指出匿名类与 Lambda 的 this 指向不同。Stream 部分说明其不是数据结构、惰性化、可并行等特性，分小节演示构造流、映射过滤、截取排序、归约统计、分组分区等常用操作。LocalDateTime 部分介绍 java.time 不可变线程安全的日期 API，演示获取、格式化、增减、比较与时区计算，并总结 DateTimeFormatter 的线程安全与格式易错点。

## 要点

- Lambda 表达式是匿名函数：参数类型可推断，单条语句可省略花括号，单参数且类型可推导时可省略圆括号。
- Lambda 可替代传统遍历与匿名内部类：map.forEach((k,v)->…)、list.forEach(System.out::println)、Runnable r=()->…。
- 匿名类的 this 指向匿名类自身，Lambda 的 this 指向写就它的外部类。
- Stream 不是数据结构、不修改底层数据；Intermediate 惰性化，Terminal 才真正遍历且流只能消费一次。
- 常用操作：map 一对一映射、flatMap 一对多、filter 过滤、limit/skip 截取、sorted 排序、distinct 去重。
- reduce 用于组合元素，iterate 生成序列，summaryStatistics 一次得到 count/min/max/sum/average。
- groupingBy 按键分组、partitioningBy 按布尔分区；parallelStream 使操作自动并行。
- java.time 新日期 API 不可变且线程安全：LocalDate/LocalTime/LocalDateTime/ZonedDateTime 分工明确，Period/ChronoUnit/Duration 算时间差。

## 提炼

> 一句话定位：JDK1.8 Lambda、Stream、LocalDateTime 用法速查

### 分节提炼

- **01 构造流**：Stream.of 支持多值与数组；Arrays.stream 从数组构造；list.stream 从集合构造
- **02 流间转换**：toArray 转数组，collect 转集合；Collectors.joining 拼字符串；一个流只能消费一次
- **03 map 映射**：map 一对一映射每个元素；String::toUpperCase 与 Integer::valueOf 方法引用；n -> n*n 计算平方
- **04 filter 过滤**：filter 按条件过滤元素；findAny().orElse 取默认值；filter + mapToInt + sum 求 id 和
- **05 flatMap 扁平化**：flatMap 一对多映射；句子按空格拆分为单词再过滤
- **06 limit 截取**：limit(n) 取前 n 条；skip(n) 扔掉前 n 个元素；limit 与 skip 可组合截取
- **07 sort 排序**：sorted 对流升序排序；随机流 limit 后可排序
- **08 peek 观察**：peek 对每个元素执行操作并返回新流；可观察 filter 与 map 前后的值
- **09 parallel 并行**：parallelStream 并行处理；并行流过滤空字符串并 count
- **10 最值去重**：mapToInt + max/min 求最长最短；distinct 去重后 sorted 排序
- **11 Match 匹配**：allMatch 全符合返回 true；anyMatch 任一符合返回 true；noneMatch 全不符合返回 true
- **12 reduce 归约**：reduce 组合元素；有起始值与无起始值求和结果不同；过滤后拼接字符串
- **13 iterate 迭代**：iterate 以种子值迭代生成元素；必须用 limit 限制大小
- **14 Supplier 流**：实现 Supplier 自定义流计算规则；Stream.generate + limit 取随机用户
- **15 分组分区**：groupingBy 按键分组；partitioningBy 按布尔条件分区
- **16 统计信息**：IntSummaryStatistics 收集统计信息；一次得到 count/min/max/sum/average
- **17 获取当前时间**：LocalDate.now 不含时分秒；LocalDateTime.now 含时分秒
- **18 取时间字段**：getYear/getMonthValue/getHour 等取字段；getDayOfYear 取年中天数
- **19 格式化**：DateTimeFormatter.ofPattern 定义格式；format 按模式输出字符串
- **20 时间增减**：plusDays/minusDays 增减天数；plusMonths/minusMonths 增减月份；withYear 指定年份
- **21 创建时间**：LocalDate.of 指定年月日创建；Month 枚举或数字均可指定月份
- **22 时间差比较**：Period.between 得年月日差；ChronoUnit 算总量差（如 49 天）；Duration 以秒纳秒建模；isAfter/isBefore 比较大小
- **23 时区计算**：Clock.system(ZoneId) 取时区时间戳；ZonedDateTime.now(zoneId) 取带时区时间；DateTimeFormatter 线程安全且可 parse/format

### 带走三句话

1. Lambda 与 Stream 把「做什么」写成表达式交给 API 遍历、过滤、聚合，代码更短但可读性会下降。
2. Stream 是惰性管道：Intermediate 只搭管线，Terminal 才触发执行，且一个流只能消费一次。
3. 日期时间一律用 java.time，类不可变线程安全，格式化用 DateTimeFormatter。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
