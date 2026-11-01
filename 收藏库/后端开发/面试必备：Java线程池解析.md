---
title: "面试必备：Java线程池解析"
url: "https://mp.weixin.qq.com/s?__biz=MzkyMzU5Mzk1NQ==&mid=2247505851&idx=1&sn=7246a8187acbd9c3564c3ed193ecc0e0"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发, Java, 面试]
lang: zh
status: ok
阅读页: "阅读页/后端开发/面试必备：Java线程池解析.html"
---

# 面试必备：Java线程池解析

> 一句话定位：Java线程池面试全解：参数、执行流程、异常处理、队列与常用线程池

## 摘要

本文以五道经典线程池面试题为线索，先讲概念：线程池可降低线程创建/销毁开销、提高响应速度并复用线程。接着讲解ThreadPoolExecutor七大核心参数、execute()执行流程与四种拒绝策略。随后梳理任务异常的四种处理：try/catch、Future.get()、UncaughtExceptionHandler、重写afterExecute；再介绍五种工作队列；最后逐一分析四种常用线程池的特点、机制与场景，并说明线程池状态流转。

## 要点

- 线程池通过复用线程降低创建/销毁开销、提高响应速度。
- ThreadPoolExecutor七大核心参数：corePoolSize、maximumPoolSize、keepAliveTime、unit、workQueue、threadFactory、handler。
- execute()执行流程：先核心线程，队列满后扩非核心线程，最大线程数满则走拒绝策略。
- 四种拒绝策略：AbortPolicy（默认抛异常）、DiscardPolicy、DiscardOldestPolicy、CallerRunsPolicy。
- 任务异常四种处理：try/catch、Future.get()、UncaughtExceptionHandler、重写afterExecute。
- 五种工作队列：ArrayBlockingQueue、LinkedBlockingQueue、DelayQueue、PriorityBlockingQueue、SynchronousQueue。
- 四种常用线程池各具特点与场景；newFixedThreadPool用无界队列，任务积压可致OOM。
- 线程池五种状态：RUNNING、SHUTDOWN、STOP、TIDYING、TERMINATED。

## 提炼

> 一句话定位：Java线程池面试全解：参数、执行流程、异常处理、队列与常用线程池

### 分节提炼

- **01 前言**：线程池是后端程序员面试高频考点；文章以五道面试题为切入点展开分析
- **02 经典面试题**：五问覆盖参数、执行机制、异常、队列、内存与使用场景；面试问题1回答核心参数并描述执行流程即可
- **03 线程池概念**：线程池是管理线程的池子；避免创建/销毁线程的类加载与GC开销；提高响应速度并复用线程
- **04 创建与参数**：用ThreadPoolExecutor构造函数创建；七大参数各司其职：核心数、最大数、存活时间、单位、队列、工厂、饱和策略；threadFactory可给线程命名便于排查问题
- **05 任务执行**：execute()流程：核心线程→队列→非核心线程→拒绝策略；非核心线程空闲超过keepAliveTime会被回收
- **06 四种拒绝策略**：AbortPolicy默认抛异常；DiscardPolicy直接丢弃、DiscardOldestPolicy丢最老；CallerRunsPolicy由调用线程执行；比喻：正式员工、外包、需求池、提需求
- **07 线程池异常处理**：任务抛RuntimeException时线程池可能吞掉异常，无法感知；execute直接try/catch；submit可用Future.get()接异常；还可用UncaughtExceptionHandler或重写afterExecute
- **08 线程池的工作队列**：ArrayBlockingQueue有界FIFO；LinkedBlockingQueue不设容量则无界，上限Integer.MAX_VALUE；SynchronousQueue不存元素，插入需等移除；DelayQueue与PriorityBlockingQueue分别用于延迟与优先级
- **09 几种常用的线程池**：newFixedThreadPool固定线程数、无界队列，任务积压会OOM；newCachedThreadPool核心0、最大Integer.MAX_VALUE，60秒回收；newSingleThreadExecutor单线程串行执行；newScheduledThreadPool支持周期/延迟调度
- **10 线程池状态**：RUNNING接收新任务并处理队列；SHUTDOWN不接新任务但处理队列；STOP中断运行中任务；TIDYING全部终止后，terminated()执行完进入TERMINATED

### 带走三句话

1. 线程池本质是管理线程的池子，复用线程以降低创建/销毁开销并提升响应速度。
2. execute按核心线程→工作队列→非核心线程→拒绝策略执行；无界队列积压任务会OOM。
3. 任务异常需主动处理：try/catch、Future.get()或UncaughtExceptionHandler。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
