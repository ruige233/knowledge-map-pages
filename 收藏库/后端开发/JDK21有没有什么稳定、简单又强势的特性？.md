---
title: "JDK21有没有什么稳定、简单又强势的特性？"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247546348&idx=1&sn=f2a8e7c5f6f858ddc6cb52644afa0430"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发]
lang: zh
status: ok
阅读页: "阅读页/后端开发/JDK21有没有什么稳定、简单又强势的特性？.html"
---

# JDK21有没有什么稳定、简单又强势的特性？

> 一句话定位：AJDK21.0.5 虚拟线程：改动小、性能强，并修复 pin 死锁问题

## 摘要

AJDK21.0.5 的 Java 虚拟线程（Loom）实现与优化：AJDK 曾自研 wisp，JDK21 起改用 Loom 与上游同步，进一步减少 pin 死锁。该版本已在 tpp 大规模应用、即将上线 polardb。使用上无需再改所有 synchronized，只改线程创建部分：单线程用 Thread.ofVirtual()，线程池换虚拟线程工厂；但不限流可能引发 FGC/OOM。文中剖析 pin 问题成因与修复方案，并介绍 jcmd 诊断工具。

## 要点

- AJDK 自研 wisp 协程，JDK21 起改用 Loom 实现以与上游一致
- AJDK21.0.5 含 openjdk21 Loom 全部内容，并减少虚拟线程 pin 死锁
- 稳定性已获验证：tpp 双十一灰度无问题、大规模应用，即将上线 polardb
- 迁移成本低：无需改所有 synchronized，只改线程创建部分
- 单线程用 Thread.ofVirtual()，线程池换 Thread.ofVirtual().factory()
- 不限流虚拟线程可能引发持续 FGC 甚至 OOM，需合理设置 parallelism/maxPoolSize
- pin 问题根因是含 C/C++ native Frame 的协程不可冻结恢复
- jcmd ThreadAndVThread.dump 可输出非挂载虚拟线程栈，ATP 可分析

## 提炼

> 一句话定位：AJDK21.0.5 虚拟线程：改动小、性能强，并修复 pin 死锁问题

### 分节提炼

- **01 虚拟线程与 Loom 背景**：虚拟线程、协程、纤程同指 Loom 轻量线程概念；Go 的 goroutine 是内置轻量线程的典型；AJDK11/8 自研 wisp，JDK21 起改用 Loom 实现以与上游同步；AJDK21.0.5 包含 openjdk21 Loom 全部内容并减少 pin 死锁
- **02 稳定性与性能**：去年双十一 tpp 灰度上线至今无问题反馈，已大规模应用；即将在 polardb 上线；实验以 ajdk21 不用协程为 baseline，对比协程/ajdk11/wisp 版本；csw 表示 context switch
- **03 线程改造要点**：无需修改所有 synchronized，只需改线程创建部分；单线程：Thread.ofVirtual().start() 替代 new Thread；线程池：核心是把工厂换成 Thread.ofVirtual().factory()；官方推荐 newVirtualThreadPerTaskExecutor，但需注意限流
- **04 pin 问题与解法**：虚拟线程、Carrier Thread、OS 线程为 N:1:1，切换免用户态/内核态；pin 指虚拟线程无法从 Carrier Thread 移除，占满后应用卡死；典型成因：类加载/初始化、synchronized、native c 调用；根因是含 C/C++ native Frame 的协程不可冻结恢复
- **05 诊断工具**：jcmd <pid>/<name> ThreadAndVThread.dump 输出虚拟线程栈；相比 openjdk 能额外输出非挂载虚拟线程的栈；ATP 平台可分析该格式日志；ATP 暂只显示锁持有与等待，不给死锁结论
- **06 结语**：虚拟线程稳定、简单、强势，鼓励升级使用；欢迎使用与反馈

### 带走三句话

1. 升级到 AJDK21.0.5：虚拟线程开箱即用，无需再改所有 synchronized。
2. 改造只动线程创建处：单线程 Thread.ofVirtual()，线程池换虚拟线程工厂。
3. 注意限流与参数：parallelism 接近 CPU 核数、放宽 maxPoolSize，避免 FGC/OOM。

### 知识卡

跨全文挖掘 11 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
