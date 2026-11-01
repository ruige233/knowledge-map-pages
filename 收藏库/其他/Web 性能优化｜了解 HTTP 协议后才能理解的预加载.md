---
title: "Web 性能优化｜了解 HTTP 协议后才能理解的预加载"
url: "https://mp.weixin.qq.com/s?__biz=Mzg4NTczNzg2OA==&mid=2247506081&idx=1&sn=9100fe9f81da7ece1c9d2e8a75e01411"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "其他"
tags: [其他, 性能优化]
lang: zh
status: ok
阅读页: "阅读页/其他/Web 性能优化｜了解 HTTP 协议后才能理解的预加载.html"
---

# Web 性能优化｜了解 HTTP 协议后才能理解的预加载

> 一句话定位：梳理 DNS 预解析到 Early Hints 的 Web 预加载技术与落地实践

## 摘要

本文系统梳理 Web 预加载技术：从 CDN 动态加速、dns-prefetch、preconnect，到 preload 与 prefetch 的优先级差异，再到 prerender 少被采用的原因；接着介绍按用户行为对下一跳页面 prefetch、mousedown 提前预载、携带 cookie 及服务端设短缓存等实践；最后结合流式渲染，讲提前返回 preconnect/preload、HTTP Link、HTTP/2 Push 与 103 Early Hints。

## 要点

- 新用户首访的 DNS 查询、TCP 建连、内容下载耗时常远超服务器响应时间，需靠预加载提前工作
- CDN 动态加速利用全球边缘节点和智能路由缩短建连与传输，并可借边缘计算直出动态内容
- dns-prefetch 提前解析域名，preconnect 提前建连，preload 高优先级立即加载关键资源，prefetch 空闲时加载未来资源
- prerender 虽能整页预渲染，但存在资源浪费、数据统计混乱和兼容性问题，实际很少使用
- 可按用户 hover、mousedown 等行为精准触发下一跳页面 prefetch，mousedown 预载约省 200ms
- prefetch 默认不带 cookie，加 credentials=include 并让服务端识别 Sec-Purpose/Purpose 设置短缓存才能复用
- 流式渲染可提前返回 preconnect/preload 片段，并借 HTTP Link、HTTP/2 Push、103 Early Hints 协议级预加载

## 提炼

> 一句话定位：梳理 DNS 预解析到 Early Hints 的 Web 预加载技术与落地实践

### 分节提炼

- **01 前言：网络开销问题**：新用户首次访问时 DNS、建连、下载耗时常远超服务器 RT；预加载可在用户需要前提前加载资源，减少等待
- **02 CDN 动态加速**：CDN 边缘节点缓存处理请求，缩短用户到服务器的物理距离；CDN 服务商实时监控网络，用智能路由选择最优路径；利用边缘计算可让用户直接从边缘节点获取动态内容
- **03 dns-prefetch 预解析**：dns-prefetch 让浏览器后台提前完成域名解析；在 HTML 顶部用 link 标签声明需预解析的域名
- **04 preconnect/preload/prefetch**：preconnect 提前完成域名解析与建连；preload 高优先级立即加载关键资源；prefetch 低优先级，空闲时下载未来可能用到的资源
- **05 prerender 预渲染**：prerender 后台下载并渲染整页，跳转时立即呈现；未命中时资源开销过大，且会影响数据统计；不同浏览器实现有差异，实际方案中很少使用
- **06 行为驱动的 prefetch**：按 hover 等行为精准预测并 prefetch 下一跳页面；在 mousedown 阶段预载可节省约 200ms 点击时间；加 credentials=include 让 prefetch 请求携带 cookie；服务端识别 Sec-Purpose 头并设置短缓存以便复用
- **07 首屏请求并行加载**：SSR 页面 prefetch 效果显著，CSR 页面主要受动态接口拖累；点击瞬间并行发起首屏接口请求，脚本执行时直接复用缓存；核心仍是 credentials 携带 cookie 与服务端设短缓存
- **08 Speculation Rules API**：声明式指示浏览器对哪些链接预取，时机更精确；可用 JS 动态增删 prefetch 规则；该 API 仍处早期阶段，未来可能扩展更多配置
- **09 流式渲染提前返回**：流式渲染用 chunked 分批下发，浏览器边收边解析；利用等待空档先返回 preconnect/preload 静态片段；可据首屏数据生成 preload 标签提前返回；HTTP header 的 Link 也能实现 preconnect
- **10 HTTP/2 Push**：服务器主动推送资源，无需客户端明确请求；Nginx 用 http2_push_preload 指令启用推送；HTTP/2 Push 很多时候由 HTTP Link 特性实现
- **11 103 Early Hints**：103 Early Hints 在最终响应头前提前下发预加载提示；作用与 HTTP Link 类似；preconnect 支持普遍，preload 的 Safari 尚未支持

### 带走三句话

1. 新用户首访的 DNS、建连、下载耗时远超服务器 RT，预加载是弥补这部分网络开销的关键
2. preload 立即加载关键资源，prefetch 空闲加载未来资源；下一跳预取要带 cookie 并设短缓存才能真正复用
3. 流式渲染可提前返回 preconnect/preload 片段，并借 HTTP Link、HTTP/2 Push、103 Early Hints 更快下发资源

### 知识卡

跨全文挖掘 11 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
