---
title: "重试注解@Retry注解"
url: "https://mp.weixin.qq.com/s?__biz=Mzg3NjU3NTkwMQ==&mid=2247531752&idx=1&sn=b2029252558c5394908d3d11ca2261c2"
source: "微信公众平台"
published: "2023-12-31"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发]
lang: zh
status: ok
阅读页: "阅读页/后端开发/重试注解@Retry注解.html"
---

# 重试注解@Retry注解

> 一句话定位：从日志到源码拆解 @Retryable 重试链路与 @Recover 兜底

## 摘要

本文以支付渠道超时重试为场景，从手写 for 循环的丑代码引出 @EnableRetry、@Retryable、@Recover 三注解。作者先跑通 Demo，再开 debug 日志结合调用栈定位到 RetryTemplate#doExecute 等入口。主链路是 while 套 try-catch，靠 RetryPolicy#canRetry 判断，默认 SimpleRetryPolicy 总调用 3 次，靠抛异常触发。@Recover 按返回值与异常类型选最相近兜底；1.3.2 起支持泛型。

## 要点

- 手写 for 循环+次数检查能实现重试，但代码重复、难维护，应抽离成通用组件。
- spring-retry 三注解：@EnableRetry 开启功能、@Retryable 给方法赋能、@Recover 兜底。
- 调源码先开 debug 日志定位 RetryTemplate#doExecute，再借调用栈找到拦截器 invoke 入口。
- 重试主链路是 while+try-catch，靠 RetryPolicy#canRetry 判断是否继续重试。
- 默认 SimpleRetryPolicy 总调用 3 次；include/exclude 可指定哪些异常触发重试。
- Spring-retry 必须靠抛异常触发重试；@Backoff 控制重试间隔，默认 1000ms。
- @Recover 非必须，靠 findClosestMatch 按返回值匹配与异常类型选最相近兜底方法。
- 1.3.1 不处理泛型，@Recover 可能选错兜底；1.3.2 起将支持泛型。

## 提炼

> 一句话定位：从日志到源码拆解 @Retryable 重试链路与 @Recover 兜底

### 分节提炼

- **01 全文**：@EnableRetry/@Retryable/@Recover 把重试抽成组件。；开 debug 日志定位 RetryTemplate#doExecute，借调用栈找 invoke。；主链路 while+try-catch；SimpleRetryPolicy 默认总调用 3 次。；@Recover 按返回值匹配与异常类型选最相近兜底；1.3.1 不处理泛型可能选错。

### 带走三句话

1. 调源码前先开 debug 日志，用日志+调用栈定位入口，比一头扎进源码更高效。
2. SimpleRetryPolicy 的 3 次是总调用次数，且是否重试受 include/exclude 异常规则约束。
3. @Recover 靠返回值匹配与异常类型选最相近兜底，1.3.1 泛型场景可能选错。

### 知识卡

跨全文挖掘 11 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
