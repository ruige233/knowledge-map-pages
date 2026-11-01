---
title: "Spring Cloud + Nacos + 负载均衡器实现全链路灰度发布实战"
url: "https://mp.weixin.qq.com/s?__biz=MzAwMTk4NjM1MA==&mid=2247520519&idx=1&sn=dbac105798e93116fd8cb0466e33ed1e"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发, Spring]
lang: zh
status: ok
阅读页: "阅读页/后端开发/Spring Cloud + Nacos + 负载均衡器实现全链路灰度发布实战.html"
---

# Spring Cloud + Nacos + 负载均衡器实现全链路灰度发布实战

> 一句话定位：用Nacos元数据version与自定义Ribbon实现全链路灰度发布

## 摘要

本文以 Spring Cloud 微服务为例介绍全链路灰度发布：用 Nacos 作注册与配置中心，以服务实例 Metadata 的 version 区分生产与灰度版本。请求进网关后，前置过滤器按开关、请求头、IP、城市、用户编号判断灰度并写入 ThreadLocal，再由重写的 Ribbon 负载均衡器按 GrayStatusEnum 过滤服务列表，后置过滤器与异常处理器清除 ThreadLocal 防泄漏。业务服务用 MVC 拦截器接收灰度请求头、OpenFeign 拦截器向下游透传，实现全链路传递。文中给出 Demo 结构与配置，演示三种灰度场景，并补充任务调度与 MQ 隔离思路。

## 要点

- 灰度发布即金丝雀发布，通过流量切分平滑过渡，初始小流量可及时发现问题。
- 核心思路：用 Nacos Metadata 的 version 值区分生产/灰度服务，调用时按版本过滤。
- 网关前置过滤器判断灰度开关与请求头/IP/城市/用户编号，将 GrayStatusEnum 写入 ThreadLocal。
- 自定义 Ribbon 负载均衡器按 ALL/PROD/GRAY 状态过滤服务列表，实现按版本路由。
- 后置过滤器和全局异常处理器清除 ThreadLocal，避免内存泄漏。
- 业务服务通过 Spring MVC 拦截器接收、OpenFeign 拦截器透传灰度标记，保证全链路一致。
- 三种演示场景：关闭灰度随机路由、开启后仅生产、命中请求头全走灰度 V2。
- 分布式任务调度与 MQ 的灰度可分别用独立执行器与双套 MQ 隔离。

## 提炼

> 一句话定位：用Nacos元数据version与自定义Ribbon实现全链路灰度发布

### 分节提炼

- **01 灰度发布概念**：灰度发布即金丝雀发布，在黑白之间平滑过渡；AB test 让部分用户先用新版本，再逐步扩大；流量切分：新服务器低权重、逐步增加请求数
- **02 组件版本**：Demo 沿用 Spring Boot 2.3.12 / Hoxton.SR12 / Alibaba 2.2.9；版本较旧，新版本需自行调整，实现思路不变
- **03 核心组件**：注册中心 Nacos、网关 SpringCloudGateway；负载均衡 Ribbon（SpringCloudLoadBalancer 类似），RPC 用 OpenFeign
- **04 实现思路**：重点在服务发现：灰度流量只路由到灰度服务；用 Nacos Metadata 的 version 值区分调用版本
- **05 Demo 工程结构**：Demo 为父工程+多模块结构；核心代码集中在 spring-cloud-starter-kerwin-gray starter
- **06 灰度核心包**：网关前置过滤器判定灰度并写入 ThreadLocal；自定义 Ribbon 规则按 ALL/PROD/GRAY 过滤服务列表；后置过滤器与异常处理器清理 ThreadLocal 防内存泄漏
- **07 业务服务透传**：MVC 拦截器接收上游灰度请求头存入 Holder；OpenFeign 拦截器把灰度标记写入下游请求头继续透传
- **08 基础信息类**：GrayStatusEnum 分 ALL/PROD/GRAY 三态；配置类区分网关开关与全局版本号；自动配置默认不加载，由 kerwin.tool.gray.load=true 开启
- **09 运行与配置**：启动网关+用户/订单各 V1/V2 共五个服务；VM 参数指定端口与 nacos metadata.version；Ribbon 规则通过 NFLoadBalancerRuleClassName 指定
- **10 效果演示**：关闭灰度时随机路由，可能跨版本；开启灰度且不命中规则时只走生产 V1；请求头 gray=gray-996 命中则全走灰度 V2
- **11 源码地址**：Gitee 仓库 spring-cloud-gray-example 可自提
- **12 遗留问题**：xxl-job 用不同执行器区分灰度任务；MQ 用两套服务端隔离生产与灰度消息；作者建议 Nginx+Lua 网关 + Nacos 灰度命名空间整体隔离

### 带走三句话

1. 全链路灰度的关键是灰度标记：网关判定后写入 ThreadLocal，再经请求头在服务间透传。
2. 服务版本用 Nacos metadata.version 区分，自定义 Ribbon 按枚举过滤实现按版本路由。
3. ThreadLocal 必须在前置后置过滤器和异常处理器中成对清理，防止内存泄漏。

### 知识卡

跨全文挖掘 10 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
