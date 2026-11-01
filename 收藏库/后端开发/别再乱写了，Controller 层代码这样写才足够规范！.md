---
title: "别再乱写了，Controller 层代码这样写才足够规范！"
url: "https://mp.weixin.qq.com/s?__biz=MzAwMTE3MDY4MQ==&mid=2652471639&idx=1&sn=b9b830b071df968457f6072e38074fc0"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发]
lang: zh
status: ok
阅读页: "阅读页/后端开发/别再乱写了，Controller 层代码这样写才足够规范！.html"
---

# 别再乱写了，Controller 层代码这样写才足够规范！

> 一句话定位：Controller 层统一返回、校验、响应的规范化改造方案

## 摘要

本文讲 Spring Boot Controller 层规范写法。先介绍请求四要素与参数接收注解，再展开：用 ResultVo 与状态码枚举统一返回 code/msg/data；用 @Validated 替代手写 if 校验，以 @RestControllerAdvice 拦截 BindException 返回校验失败码；用 ResponseBodyAdvice 自动包装响应并以注解豁免特殊接口；最后用 APIException 统一业务异常，逐步改成统一可维护的规范写法。

## 要点

- 完整后端请求由接口地址、请求方式、请求数据、响应数据四部分组成。
- 用 ResultVo（code/msg/data）统一返回结构，状态码用 StatusCode 枚举定义。
- 状态码枚举用 @Getter 而非 @Data，避免生成 setter。
- @Validated 配合 @NotNull/@Min 等字段注解替代手写 if 校验。
- @RestControllerAdvice + @ExceptionHandler 拦截 BindException，统一返回 1002 校验失败码。
- ResponseBodyAdvice 自动包装响应，String 类型需 ObjectMapper 特殊处理。
- @NotControllerResponseAdvice 注解豁免健康检测等不需要包装的接口。
- APIException + AppCode 统一业务异常，抛异常即自动返回状态码并打日志。

## 提炼

> 一句话定位：Controller 层统一返回、校验、响应的规范化改造方案

### 分节提炼

- **01 Controller 参数接收**：完整请求由 URL、请求方式、请求数据、响应数据四要素组成；@RestController=@Controller+ResponseBody，@RequestMapping 为类级前缀；@GetMapping/@PostMapping 标记 get/post，VO 参数由 JSON 自动映射
- **02 统一状态码**：返回统一包装 code/msg/data，约定 1000 为成功；状态码用 StatusCode 接口 + ResultCode 枚举定义，不写死常量；ResultVo 提供默认成功、指定状态码、只返回状态码等构造方法
- **03 统一参数校验**：手写 if 校验冗长，改用 @Validated 加 @NotNull/@Min 注解；校验失败抛 BindException，默认返回 400 结构不符合约定；@RestControllerAdvice 拦截 BindException，包装成 1002 校验失败码
- **04 统一响应包装**：ResponseBodyAdvice 在响应时统一包装成 ResultVo；String 返回类型需用 ObjectMapper 转 JSON 特殊处理；supports 过滤已是 ResultVo 或带豁免注解的方法
- **05 统一异常处理**：AppCode 业务状态码实现 StatusCode 接口；APIException 区分 code（状态码）、msg（大类）、message（详情）；统一拦截 APIException，业务代码只抛异常不关心返回与日志

### 带走三句话

1. 返回结构统一为 code/msg/data，状态码用枚举定义并与前端约定好。
2. 参数校验用 @Validated 加注解，异常交给 @RestControllerAdvice 统一包装。
3. 业务异常统一抛 APIException，响应包装与日志集中在 Advice 中处理。

### 知识卡

跨全文挖掘 11 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
