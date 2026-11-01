---
title: "Spring Boot 实现各种参数校验，写得太好了，建议收藏！"
url: "https://mp.weixin.qq.com/s?__biz=MzA5MTkxMDQ4MQ==&mid=2648947501&idx=1&sn=f865ecb295e39c29121d6938b2c78383"
source: "微信公众平台"
published: "2023-11-13"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发, Spring]
lang: zh
status: ok
阅读页: "阅读页/后端开发/Spring Boot 实现各种参数校验，写得太好了，建议收藏！.html"
---

# Spring Boot 实现各种参数校验，写得太好了，建议收藏！

> 一句话定位：Spring Validation 参数校验实战与底层实现原理全解

## 摘要

本文讲解 Spring Boot 参数校验的使用与原理。简单使用厘清 JSR303、hibernate-validation 与 Spring Validation 的关系，演示 requestBody 与 requestParam 两种校验及异常，用 @RestControllerAdvice 统一处理。进阶使用覆盖分组、嵌套、集合、自定义、编程式校验和 Fail Fast。实现原理说明两类校验分别由参数解析器与 AOP 触发，最终都委托 Hibernate Validator 执行。

## 要点

- Spring Validation 是对 hibernate-validation 的二次封装，底层真正执行校验的是 Hibernate Validator。
- requestBody 校验：DTO 对象加 @Validated/@Valid，失败抛 MethodArgumentNotValidException，默认转 400。
- requestParam/PathVariable 校验：Controller 类上加 @Validated、入参声明约束注解，失败抛 ConstraintViolationException。
- 用 @RestControllerAdvice + @ExceptionHandler 统一处理两类校验异常，可按需让 HTTP 状态码返回 200、由业务码区分。
- 同一 DTO 多场景校验规则不同时，用分组校验 groups（如 Save/Update）区分。
- 嵌套对象字段必须加 @Valid 才生效；直接 List/Set 接收 JSON 数组校验不生效，需包装 List 并加 @Valid。
- 自定义校验分两步：自定义约束注解 + 实现 ConstraintValidator 编写校验器。
- 方法级校验本质是 AOP：MethodValidationPostProcessor 注册切面、MethodValidationInterceptor 织入增强。

## 提炼

> 一句话定位：Spring Validation 参数校验实战与底层实现原理全解

### 分节提炼

- **01 简单使用**：Spring Validation 是 hibernate-validation 的二次封装，底层由 Hibernate Validator 执行校验。；spring-boot 2.3.x 起需手动引入 hibernate-validator 依赖。；requestBody 用 DTO+@Validated/@Valid；GET 平铺参数需类上加 @Validated 且入参声明约束注解。；校验失败分别抛 MethodArgumentNotValidException 与 ConstraintViolationException，用 @RestControllerAdvice 统一处理。
- **02 进阶使用**：同一 DTO 多个方法校验规则不同，用分组校验 groups 区分。；嵌套校验时对象字段必须加 @Valid，且可与分组校验结合。；直接 List/Set 接收 JSON 数组校验不生效，需包装 List 并加 @Valid。；自定义校验分两步：自定义约束注解 + 实现 ConstraintValidator；可注入 Validator 编程式校验，配置 failFast(true) 开启快速失败。
- **03 实现原理**：requestBody 校验在 RequestResponseBodyMethodProcessor.resolveArgument() 的 validateIfApplicable() 中触发。；方法级校验基于 AOP：MethodValidationPostProcessor 注册切面、MethodValidationInterceptor 织入增强。；两类校验最终都委托 Hibernate Validator 执行，Spring Validation 只是封装。

### 带走三句话

1. Spring Validation 只是封装，真正校验由 Hibernate Validator 执行，看注解与校验器。
2. 按传参选姿势：requestBody 用 DTO+@Validated，GET 平铺参数在类上加 @Validated。
3. 复杂场景记三点：嵌套字段加 @Valid、JSON 数组用包装 List、多规则用分组校验。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
