---
title: "奇淫巧技，CompletableFuture 异步多线程是真的优雅"
url: "https://mp.weixin.qq.com/s?__biz=MzA4NzQ0Njc4Ng==&mid=2247506966&idx=1&sn=8651f469cf20b8f25fdc4e636cbca366"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发]
lang: zh
status: ok
阅读页: "阅读页/后端开发/奇淫巧技，CompletableFuture 异步多线程是真的优雅.html"
---

# 奇淫巧技，CompletableFuture 异步多线程是真的优雅

> 一句话定位：一文讲透 CompletableFuture 的创建、回调、组合与结果获取

## 摘要

本文介绍 Java 8 的 CompletableFuture 异步编程。先以 Future 加 CountDownLatch 的例子说明传统异步获取结果需阻塞或轮询、多任务依赖难处理，再用 CompletableFuture 重写同样场景。随后讲解四种创建方式 supplyAsync/runAsync（各支持默认或自定义线程池）与四种结果获取方式 get、get(超时)、getNow、join。接着展开链式回调 thenRun、thenAccept、thenApply 及 Async 后缀的线程池差异，以及异常回调 whenComplete 与 exceptionally 的组合。再介绍多任务组合：AND 关系 thenCombine/thenAcceptBoth/runAfterBoth、OR 关系 applyToEither/acceptEither/runAfterEither，以及 allOf/anyOf。最后给出四点注意事项：需调用 get 才能看到异常、get 阻塞要加超时、慎用默认 ForkJoin 线程池、自定义线程池建议 AbortPolicy。

## 要点

- Future 需 get() 阻塞或轮询 isDone 获取结果，方式不优雅且难以处理任务依赖。
- CompletableFuture 用 supplyAsync/runAsync 创建异步任务，前者有返回值、后者无返回值，均可自定义线程池。
- 结果获取有 get、get(超时)、getNow、join 四种，join 不抛受检异常但结果异常抛 CompletionException。
- 链式回调 thenRun/thenAccept/thenApply 支持任务串行，Async 后缀决定是否改用默认 ForkJoin 池。
- whenComplete 正常或异常都回调，配 exceptionally 可捕获异常并给默认返回值。
- AND 组合等两任务都完成，OR 组合任一任务完成即执行，allOf/anyOf 支持多任务协同。
- get() 阻塞需加超时、慎用默认线程池、自定义池建议 AbortPolicy 并做好线程池隔离。

## 提炼

> 一句话定位：一文讲透 CompletableFuture 的创建、回调、组合与结果获取

### 分节提炼

- **01 4种创建方式**：supplyAsync 执行 Supplier 任务，支持返回值；runAsync 执行 Runnable，无返回值；两种方法都有重载：不传 Executor 用默认 ForkJoinPool.commonPool()，传则用自定义线程池
- **02 4种结果获取**：get() 与 get(timeout, unit) 来自 Future，后者超时抛异常；getNow(valueIfAbsent) 立即返回：已完成返回结果，未完成返回默认值；join() 方法内不抛异常，但结果异常抛 CompletionException；get() 抛 ExecutionException
- **03 thenRun 回调**：thenRun：前一任务完成后执行第二任务，第二任务无入参、无返回值；传入自定义线程池时：thenRun 与第一任务共用线程池；thenRunAsync 第二任务用 ForkJoin 池
- **04 thenAccept 回调**：thenAccept 将上一任务结果作为入参传入回调，回调无返回值；thenAccept 与 thenAcceptAsync 的区别同 thenRun 系列：线程池选择不同
- **05 thenApply 回调**：thenApply 将上一任务结果作为入参，回调有返回值，可链式转换结果；thenApply 与 thenApplyAsync 的区别同 thenRun 系列
- **06 whenComplete 回调**：whenComplete 无论正常完成还是出现异常都会被调用；正常完成：结果为上级任务结果、异常为 null；出现异常：结果为 null、异常为上级异常
- **07 whenComplete+exceptionally**：exceptionally 可捕获上游异常并返回默认值，避免 get() 抛异常；组合使用时出现异常：whenComplete 打印 null 结果与异常，exceptionally 兜底返回 0.0
- **08 AND 组合**：thenCombine/thenAcceptBoth/runAfterBoth 都是两任务都完成再执行任务三；runAfterBoth 无入参无返回值；thenAcceptBoth 传入两结果无返回值；thenCombine 传入两结果有返回值
- **09 OR 组合**：applyToEither/acceptEither/runAfterEither 都是任一任务完成即执行任务三；三者区别同 AND 系：是否传入已完成任务结果、是否有返回值；线程池核心线程数设为 1 时，任务三可能被直接丢弃不执行
- **10 allOf/anyOf**：allOf 等待所有任务完成；anyOf 只要有一个任务完成即返回；allOf 返回 Void，需再对各任务 get() 取结果；anyOf 返回完成任务的 Object 结果
- **11 异常须靠 get 暴露**：不调用 get()/join() 获取返回值，异步任务中的异常不会被看到；建议加 try...catch 或使用 exceptionally 处理异常
- **12 get 阻塞注意**：get() 阻塞获取结果，应添加超时时间如 get(5, TimeUnit.SECONDS)
- **13 慎用默认线程池**：默认 ForkJoin 线程池线程数为 CPU 核数-1，高并发复杂逻辑响应慢；建议使用自定义线程池并优化配置参数
- **14 线程池饱和策略**：建议线程池拒绝策略使用 AbortPolicy；DiscardPolicy/DiscardOldestPolicy 饱和时会直接丢任务且不抛异常；耗时的异步线程要做好线程池隔离

### 带走三句话

1. 用 supplyAsync/runAsync 创建异步任务，链式 thenApply/thenAccept/thenRun 串起依赖，告别 CountDownLatch。
2. 获取结果优先 get(超时) 或 join；异常处理用 whenComplete + exceptionally 兜底。
3. 生产环境自定义线程池并选 AbortPolicy，避免默认 ForkJoin 池与静默丢任务。

### 知识卡

跨全文挖掘 13 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
