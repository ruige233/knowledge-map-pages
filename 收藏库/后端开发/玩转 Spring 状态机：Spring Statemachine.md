---
title: "玩转 Spring 状态机：Spring Statemachine"
url: "https://mp.weixin.qq.com/s?__biz=MzU4MDUyMDQyNQ==&mid=2247551857&idx=1&sn=0adf981718e4e25658248bb41e5041c6"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发, Spring]
lang: zh
status: ok
阅读页: "阅读页/后端开发/玩转 Spring 状态机：Spring Statemachine.html"
---

# 玩转 Spring 状态机：Spring Statemachine

> 一句话定位：用 Spring Statemachine 实现订单状态流转控制

## 摘要

以订单状态管理为例介绍在 Spring Boot 中用 Spring Statemachine 实现状态流转控制。先述框架特点：灵活配置、支持并发与嵌套状态、易集成 Spring 生态；再分析五种状态及转换关系。实现步骤：引入依赖、枚举定义状态与事件、StateMachineConfigurerAdapter 配置转换与监听器、@WithStateMachine 与 @OnTransition 处理后置业务、业务层发事件触发、控制层暴露接口。最后验证：非法流转被拒、合法流转成功、重复触发报错。

## 要点

- Spring Statemachine 是 Spring 团队提供的轻量级状态机框架，适用于订单处理、工作流引擎等状态管理场景。
- 框架特点：灵活状态配置、支持并发与嵌套状态、与 Spring 生态良好集成。
- 订单状态流转：NEW→PAID→SHIPPED→COMPLETED，且 NEW、PAID 可被取消为 CANCELLED。
- 用枚举定义状态 OrderStates 和事件 OrderEvents，通过 StateMachineConfigurerAdapter 配置状态、转换、监听器与内存持久化。
- 用 @WithStateMachine 与 @OnTransition 注解实现状态改变后的业务处理，如更新订单状态。
- 业务层用 MessageBuilder 构建带 order 头的消息并 sendEvent 触发；控制层用 @GetMapping 暴露接口。
- 测试验证：非法流转 NEW→SHIPPED/COMPLETE 被拒绝，合法流转成功，重复触发同一流转会报错。
- 适用场景不仅订单处理，还可用于各种需要状态管理的业务流程。

## 提炼

> 一句话定位：用 Spring Statemachine 实现订单状态流转控制

### 分节提炼

- **01 框架概述**：Spring 团队提供的轻量级状态机框架，管理复杂状态流转逻辑；特点：灵活状态配置、支持并发与嵌套状态、与 Spring 生态良好集成
- **02 场景分析**：订单五种状态：NEW、PAID、SHIPPED、COMPLETED、CANCELLED；NEW→PAID→SHIPPED→COMPLETED，NEW、PAID 可取消
- **03 原理与实现**：状态和状态转换通过配置定义；每个转换由事件触发，驱动状态流转
- **04 引入依赖**：引入 spring-statemachine-starter 依赖；用 Maven 坐标引入该 starter
- **05 定义状态和事件**：枚举 OrderStates 定义五个订单状态；枚举 OrderEvents 定义 PAY、SHIP、COMPLETE、CANCEL 四个事件
- **06 配置状态机**：StateMachineConfigurerAdapter 配置状态、事件、监听器和持久化；initial 设为 NEW，end 设为 COMPLETED 与 CANCELLED；转换用 withExternal().source().target().event() 链式定义；监听器记录变更与错误事件，内存持久化用 HashMap
- **07 后置处理器**：@WithStateMachine 标记处理器，@OnTransition 绑定转换；在转换后更新订单状态、可扩展保存订单等业务
- **08 业务层使用**：MessageBuilder 构建带 order 头的 Message；sendEvent(Mono.just(message)).subscribe() 触发流转
- **09 控制层触发**：@GetMapping 暴露 pay、ship、complete 三个接口；各接口调用 orderService 对应方法触发转换
- **10 原理解析**：核心是有限状态机模型，用状态、事件、转换控制业务流程；状态表示阶段、事件触发转换，支持嵌套状态与并发状态
- **11 测试**：配置完成后通过接口调用验证状态机是否达到预期；按 NEW、PAID、SHIPPED、COMPLETED 的顺序逐项验证
- **12 测试一**：NEW 只能转 PAID 或 CANCELLED，NEW→SHIPPED 不成功；控制台打印错误信息，处理器业务未执行
- **13 测试二**：与上一例相同，期望报错且不能转换成功；确认非法流转统一被拒绝
- **14 测试三**：NEW 可转 PAID，流转成功并进入 handler 更新订单为 PAID；再次支付会报错：订单已是 PAID，不能重复 NEW→PAY
- **15 测试四**：PAID→SHIPPED 符合配置，能成功流转；再次调用报错：订单已是 SHIPPED，不能重复转换
- **16 测试五**：SHIPPED 可转换为 COMPLETED，结果与预期一致；按配置完成最后一步流转
- **17 总结**：Spring Statemachine 可高效管理订单等流程的状态流转；不仅订单处理，也适用于各种需要状态管理的场景

### 带走三句话

1. 状态、事件、转换三要素用配置声明，枚举定义状态与事件，由事件驱动状态流转。
2. StateMachineConfigurerAdapter 配转换与监听器，@OnTransition 处理业务。
3. 非法流转被拒、重复流转报错，用测试验证状态机配置。

### 知识卡

跨全文挖掘 9 张知识卡（代码模式 / 数据 / 易错点 / 结论），带原文锚点，完整内容见阅读页提炼层。
