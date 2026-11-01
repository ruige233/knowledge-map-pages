---
title: "讲的太通透了，切面 AOP 优雅的实现权限校验！"
url: "https://mp.weixin.qq.com/s?__biz=MzU4MDUyMDQyNQ==&mid=2247519262&idx=1&sn=3d6e105a9a0780bccdab395221df4869"
source: "微信公众平台"
published: "2023-10-01"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发]
lang: zh
status: ok
阅读页: "阅读页/后端开发/讲的太通透了，切面 AOP 优雅的实现权限校验！.html"
---

# 讲的太通透了，切面 AOP 优雅的实现权限校验！

> 一句话定位：Spring AOP 从概念到实战：自定义注解+切面优雅实现权限校验

## 摘要

本文系统讲解 Spring AOP。先说明 AOP 是 Spring 三大核心思想之一，权限校验、日志等非业务代码散落业务逻辑中造成冗余，抽成公共方法仍须手动调用，而 AOP 可将横切逻辑分离并自动织入；再梳理 Pointcut、Advice、Aspect、Joint point、Weaving 五个核心概念。接着用两个实例演示：一是 @Pointcut 拦截 @GetMapping 输出日志，二是自定义注解 @PermissionsAnnotation 配合 @Around 切面读取入参做权限校验，并用 @Order 管理多切面执行顺序。最后详解 @Pointcut、@Around、@Before、@After 等注解用法与易错点。

## 要点

- AOP 把权限校验、日志记录等非业务代码抽离，与业务代码分离后按切入点自动织入。
- AOP 要做三类事：在哪切入、何时切入、切入后做什么。
- 核心概念：Pointcut（切点）、Advice（处理）、Aspect（切面）、Joint point（连接点）、Weaving（织入）。
- 切点表达式分 execution() 路径方式与 annotation() 注解方式，自定义注解也可作为切点。
- 自定义注解 @PermissionsAnnotation 配合 @Around 切面，读取 joinPoint.getArgs() 做权限校验。
- 多个切面类执行顺序由 @Order 管理，数字越小越先执行。
- @Around 通过 ProceedingJoinPoint.proceed() 控制目标方法执行，可修改入参；@Before/@After 等注解负责方法前后处理。

## 提炼

> 一句话定位：Spring AOP 从概念到实战：自定义注解+切面优雅实现权限校验

### 分节提炼

- **01 理解AOP**：AOP 是 Spring 三大核心思想之一，另两个是 IOC（控制反转）、DI（依赖注入）。；权限校验、日志等横切需求散落业务代码中，冗余且不利于维护。；抽成公共方法仍需逐一手动调用，AOP 则自动寻找节点切入业务代码。；切点分 execution 与 annotation 两种方式，连接点在 Spring AOP 中代表方法执行。
- **02 AOP实例**：引入 spring-boot-starter-aop 依赖，用 @Aspect + @Component 定义切面类。；自定义注解作切点，@Pointcut 拦截标注该注解的方法。；多个切面类用 @Order 管理顺序，数字越小越先执行。；Around 方法第一个形参必须是 ProceedingJoinPoint，调 proceed 才执行目标方法。
- **03 AOP相关注解**：@Pointcut 支持 execution() 路径与 annotation() 注解两种表达式。；@Around 可改参数和返回值、控制执行时机，但需线程安全环境。；@Before/@After 在方法前后执行，可记录日志、URL、IP。；@AfterReturning/@AfterThrowing 分别捕获返回值与异常做增强。

### 带走三句话

1. 用自定义注解 + @Around 切面做权限校验：切点拦注解、getArgs() 取参判断、放行时调 proceed()。
2. 多个切面类用 @Order 控制执行顺序，数字越小越先执行。
3. @Around 需在线程安全环境使用；@AfterReturning/@AfterThrowing 的 returning/throwing 值须与参数名一致。

### 知识卡

跨全文挖掘 11 张知识卡（代码模式 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
