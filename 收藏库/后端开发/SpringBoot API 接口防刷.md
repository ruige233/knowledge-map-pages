---
title: "SpringBoot API 接口防刷"
url: "https://mp.weixin.qq.com/s?__biz=Mzg3MzY2OTcyMQ==&mid=2247486093&idx=1&sn=99bd2611f48afa957cf5ea599590a9bd"
source: "微信公众平台"
published: "2023-10-01"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发, Spring]
lang: zh
status: ok
阅读页: "阅读页/后端开发/SpringBoot API 接口防刷.html"
---

# SpringBoot API 接口防刷

> 一句话定位：用 Redis 计数 + 自定义注解拦截器实现 SpringBoot 接口限流防刷

## 摘要

本文介绍 SpringBoot 接口防刷：限制某用户在某时间段内对某接口的请求次数。原理是服务器用 Redis 记录请求次数，超限即拒绝，key 带时效、过期自动删除。实现上以自定义注解 @RequestLimit 声明 second 秒内最多 maxCount 次，拦截器 RequestLimitIntercept 读取方法或类注解（方法优先），以 servletPath 加 sessionId 为 key 写入或自增 Redis 计数，超限回写 JSON 错误；经 WebMvcConfig 注册后，在 Controller 类或方法上加注解即可使用。

## 要点

- 接口防刷目标：某用户在某时间段内对某接口只能请求 N 次。
- 原理：Redis 记录请求次数，超过上限拒绝访问，key 带过期时间自动删除。
- 自定义注解 @RequestLimit：声明 second 秒内最多 maxCount 次，可作用于方法或类。
- 拦截器 RequestLimitIntercept 继承 HandlerInterceptorAdapter，在 preHandle 中做限流校验。
- 方法注解优先于类注解，二者同时存在时以方法参数为准。
- limitKey 由 servletPath 加 sessionId 组成（浏览器测试），App 场景可换用户 ID。
- SpringBoot 2.x 实现 WebMvcConfigurer，在 addInterceptors 中注册拦截器。
- 超限时以 UTF-8 + application/json 回写 Result.error 错误信息。

## 提炼

> 一句话定位：用 Redis 计数 + 自定义注解拦截器实现 SpringBoot 接口限流防刷

### 分节提炼

- **01 全文**：接口防刷即限制某用户某时间段内对某接口的请求次数，超过上限拒绝访问。；@RequestLimit 注解以 second 秒内最多 maxCount 次声明限流参数，可加在类或方法上。；RequestLimitIntercept 用 Redis 计数，方法注解优先于类注解，超限回写 JSON 错误。；SpringBoot 2.x 实现 WebMvcConfigurer，并在 addInterceptors 中注册自定义拦截器。

### 带走三句话

1. 接口防刷的核心是 Redis 计数加过期时间，超限即拒绝访问。
2. 用 @RequestLimit 声明限流参数，拦截器读注解并用 Redis 自增计数，方法注解优先于类注解。
3. SpringBoot 2.x 实现 WebMvcConfigurer 注册拦截器，类或方法上加注解即可启用限流。

### 知识卡

跨全文挖掘 7 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
