---
title: "FullGC 每天40次 到 10天1次，真牛B！！"
url: "https://mp.weixin.qq.com/s?__biz=MzkxNTU5NDcxMg==&mid=2247501816&idx=1&sn=ebca3af2ed92e694f786be5400ec4e03"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发]
lang: zh
status: ok
阅读页: "阅读页/后端开发/FullGC 每天40次 到 10天1次，真牛B！！.html"
---

# FullGC 每天40次 到 10天1次，真牛B！！

> 一句话定位：JVM FullGC 从每天 40 次降到近 10 天一次的实战调优复盘

## 摘要

作者复盘线上 JVM FullGC 调优：四台 2 核 4G 机器每天 FullGC 40 多次并偶发重启。第一次只调参数，新生代 350M→800M、初始堆对齐最大堆，YoungGC 减半但 FullGC 反增 41 次而失败。随后查内存泄漏：先发现 redis 匿名内部类 listener 持有对象 T 不释放；再 dump 抓到 4W+ ByteArrowRow 对象，定位到查询漏加 module 条件、一次查 40 多万条，入口流量一度 83MB/s。修复后 FullGC 骤降。第二次调优把 metaspace 从默认 21M 提到 200M 并调 CMS 参数，prod1（-Xmn800M）最优。最终 FullGC 近 10 天一次，YoungGC 时间减半。

## 要点

- 线上 2 核 4G 服务器 FullGC 平均每天 40 多次并伴随自动重启，成为调优起点。
- 第一次调优加大新生代并对齐初始/最大堆后，YoungGC 减半但 FullGC 反而增加 41 次。
- 对象 T 存在上万实例约 20M：redis listener 匿名内部类持有引用、超时不释放导致泄漏。
- dump 抓到 4W+ ByteArrowRow 对象，定位到查询漏加 module 条件、一次查出 40 多万条数据。
- 修复泄漏后用原参数运行 3 天仅 5 次 FullGC，确认根因在查询而非 JVM 参数。
- 第二次调优把 metaspace 提到 200M、CMSInitiatingOccupancyFraction=75，prod1（-Xmn800M）最优。
- 最终 FullGC 从每天 40 次降到近 10 天一次，YoungGC 时间减少一半以上。
- 总结：FullGC 一天超过一次即不正常，优先排查泄漏，再考虑参数调优。

## 提炼

> 一句话定位：JVM FullGC 从每天 40 次降到近 10 天一次的实战调优复盘

### 分节提炼

- **01 全文**：FullGC 频繁时应先查内存泄漏，本文两处泄漏分别是匿名内部类 listener 与查询漏条件。；仅调 JVM 参数有风险：第一次优化让 YoungGC 减半，FullGC 反而增加 41 次。；metaspace 默认 21M 过小，峰值约 200M 时老年代不足 30% 也会触发 FullGC。；CPU 100% 也可能是服务器本身问题，可找阿里云客服确认后迁移服务器。

### 带走三句话

1. FullGC 频繁先别急着调参数，优先排查内存泄漏，对象引用与查询漏条件都是常见根因。
2. metaspace 过小会让老年代不足 30% 就触发 FullGC，需按实际峰值调大 MetaspaceSize。
3. 泄漏修复后参数调优空间很小，但持续关注 GC 能及早发现服务器异常。

### 知识卡

跨全文挖掘 9 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
