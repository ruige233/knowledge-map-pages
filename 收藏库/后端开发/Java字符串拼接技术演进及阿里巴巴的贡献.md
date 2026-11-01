---
title: "Java字符串拼接技术演进及阿里巴巴的贡献"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247540622&idx=1&sn=06610c8ac15bd6748bf21ad1a022efae"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发, Java]
lang: zh
status: ok
阅读页: "阅读页/后端开发/Java字符串拼接技术演进及阿里巴巴的贡献.html"
---

# Java字符串拼接技术演进及阿里巴巴的贡献

> 一句话定位：Java字符串拼接演进史：从StringBuilder到阿里的PR 20273

## 摘要

本文梳理Java字符串拼接实现演进。JDK 8中"+"编译为StringBuilder调用，非循环体内与手写StringBuilder性能一致。JDK 9起改用StringConcatFactory动态生成拼接方法，不建对象、少一次数组拷贝，JDK 11下"+"比StringBuilder快10.97%。但MethodHandle实现参数多时中间类多、启动慢，C2编译需2G内存。阿里PR 20273用隐藏类字节码替代MethodHandle，提升启动性能、降低C2开销，将成JDK 24缺省实现。

## 要点

- JDK 8中"+"拼接被编译为StringBuilder调用，非循环体内与直接使用StringBuilder性能一致。
- JDK 9+（JEP 280）改用invokedynamic + StringConcatFactory.makeConcatWithConstants动态生成拼接方法。
- 新实现不创建StringBuilder对象、减少一次数组拷贝，JDK 11下"+"比StringBuilder快约10.97%。
- MethodHandle表达式在参数多时中间类多、启动慢，C2编译极端需高达2G内存。
- 阿里巴巴温绍锦提交PR 20273：用动态字节码生成隐藏类替代MethodHandle，将成为JDK 24缺省实现。
- PR 20273用非静态字段保存constants实现重用，coder与length不再合并，便于C2内联优化。
- 阿里另贡献simpleConcat、Float/Double toString、Integer/Long.toString等多项OpenJDK性能优化。

## 提炼

> 一句话定位：Java字符串拼接演进史：从StringBuilder到阿里的PR 20273

### 分节提炼

- **01 省流版**：本文用一张图概览Java字符串拼接技术演进路线；最新实现PR 20273来自阿里巴巴贡献
- **02 关于使用+拼接**：旧文说+拼接性能差的结论已过时，JDK 9+的+比StringBuilder快；示例方法concatIndy用于对比JDK 8与JDK 9+的实现
- **03 JDK 8实现**：JDK 8中+编译为new StringBuilder().append().toString()；非循环体内+与StringBuilder一致，代码更简洁推荐用+
- **04 JEP 280实现**：JDK 11中+编译为invokedynamic调用makeConcatWithConstants；动态生成方法不创建StringBuilder、少一次数组拷贝；JMH显示JDK 11下+比StringBuilder快10.97%
- **05 实现细节与阿里贡献**：MethodHandle实现参数多时中间类多、启动慢，C2编译极端需2G内存；JDK 23引入highArityThreshold默认20，超阈值回退StringBuilder；PR 20273用隐藏类动态字节码替代MethodHandle，兼顾性能与重用；PR 20273将成为JDK 24缺省实现
- **06 阿里其他贡献**：PR 20253优化simpleConcat单参数拼接性能约8%；PR 19730消除浮点toString内存分配，性能提升约48%~88%；PR 14699提升Integer/Long.toString及append性能
- **07 总结**：JDK 8中+与StringBuilder性能一致，升级到JDK 11后+更快且GC更少；JDK 11~23参数多时拼接存在启动慢、C2资源消耗高的问题；PR 20273解决上述问题，将成JDK 24的+运算缺省实现

### 带走三句话

1. JDK 9+ 直接用+拼接即可，不必再迷信手写StringBuilder。
2. 参数极多的拼接在JDK 11~23启动慢，可关注JDK 24的PR 20273修复。
3. 升级JDK时把compiler.target同步升到新版本，才能获得新拼接实现的红利。

### 知识卡

跨全文挖掘 10 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
