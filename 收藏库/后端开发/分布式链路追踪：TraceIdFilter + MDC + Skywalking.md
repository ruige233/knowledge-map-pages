---
title: "分布式链路追踪：TraceIdFilter + MDC + Skywalking"
url: "https://mp.weixin.qq.com/s?__biz=MzIyNDU2ODA4OQ==&mid=2247515398&idx=1&sn=d8bdc5a11aa454656e085f179fcc6ae4"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发, 分布式]
lang: zh
status: ok
阅读页: "阅读页/后端开发/分布式链路追踪：TraceIdFilter + MDC + Skywalking.html"
---

# 分布式链路追踪：TraceIdFilter + MDC + Skywalking

> 一句话定位：TraceId+MDC+Skywalking 打通跨线程跨服务日志追踪

## 摘要

文章给出 TraceId + MDC + Skywalking 的分布式链路追踪落地方案。先点明多线程日志交错、多 Pod 日志汇聚后难以按请求定位的痛点；随后分三步实现单服务内追踪：前端生成 X-App-Trace-Id 请求头，后端 TraceIdFilter 提取、缺失时自行生成 traceId 并写入 slf4j MDC，logback pattern 用 %X{traceId} 打印。跨服务由 Feign 拦截器把 traceId 塞回请求头透传；多线程场景因子线程不会自动继承 MDC，需在线程池执行器、TaskDecorator 与装饰器中复制并清理上下文。整合 Skywalking 使用 apm-toolkit-logback-1.x 的 TraceIdPatternLogbackLayout，%tid 打印 Skywalking traceId，并靠 agent 重写写死 TID:N/A 的 converter。最后解析 MDC 的 MDCAdapter、ThreadLocal 实现与 logback 占位符 Converter 机制。

## 要点

- 痛点：同一 Pod 内多线程日志交错，多 Pod 日志汇聚后更难按请求定位。
- 前端每次请求添加 X-App-Trace-Id 头，值用时间戳 + UUID 保证唯一性。
- 后端 TraceIdFilter 取出请求头，缺失时用 UUID 或 Snowflake 生成 traceId。
- traceId 写入 slf4j MDC，logback pattern 用 %X{traceId} 打印，Filter finally 中 MDC.remove 防串。
- Feign RequestInterceptor 把 MDC 中的 traceId 回填请求头，实现跨服务透传。
- 子线程不会自动继承父线程 MDC，需在线程池、TaskDecorator 与装饰器中复制并清理上下文。
- Skywalking 用 apm-toolkit-logback-1.x 的 TraceIdPatternLogbackLayout 提供 %tid/%sw_ctx，agent 重写写死 TID:N/A 的 converter。
- MDC 是 slf4j 规范，落到 MDCAdapter；LogbackMDCAdapter 基于 ThreadLocal，%X/%mdc 由 MDCConverter 转为 MDC.get(key)。

## 提炼

> 一句话定位：TraceId+MDC+Skywalking 打通跨线程跨服务日志追踪

### 分节提炼

- **01 全文**：方案主线：前端带 X-App-Trace-Id → Filter 写入 MDC → Feign 透传 → 线程适配 → Skywalking 关联。；核心范式：Filter 里 put 后 finally remove；拦截器回填请求头；装饰器复制并清理子线程 MDC。；Skywalking 的 %tid 依赖 agent 改写写死 TID:N/A 的 LogbackPatternConverter。；logback 的 %X{key} 由 MDCConverter 解析 key，最终调用 MDC.get(key) 取值。

### 带走三句话

1. 每个请求一个 traceId：前端带头、Filter 兜底生成并写入 MDC，finally 清理防串。
2. 跨线程跨服务都要显式传递：Feign 回填请求头，线程池与 TaskDecorator 复制/清理子线程 MDC。
3. Skywalking 的 %tid 靠 agent 改写写死 TID:N/A 的 converter，启动必须挂 -javaagent。

### 知识卡

跨全文挖掘 8 张知识卡（代码模式 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
