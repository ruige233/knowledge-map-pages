---
title: "【线程池】Java线程池的内部类Worker详解_线程池worker-CSDN博客"
url: "https://blog.csdn.net/cy973071263/article/details/131484384"
source: "CSDN"
published: "2026-11-01"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发, Java]
lang: zh
status: ok
阅读页: "阅读页/后端开发/【线程池】Java线程池的内部类Worker详解_线程池worker-CSDN博客.html"
---

# 【线程池】Java线程池的内部类Worker详解_线程池worker-CSDN博客

> 一句话定位：逐行拆解线程池内部类 Worker 的锁、属性与执行流程

## 摘要

本文拆解 ThreadPoolExecutor 内部类 Worker 的实现。Worker 封装线程池线程：持有 thread 与 firstTask，用 completedTasks 统计完成数；它继承 AQS 实现不可重入独占锁，构造时 state 置 -1，加锁表示正在执行任务。线程启动后 run() 委托 runWorker()，循环执行 firstTask 或经 getTask() 取任务，超时返回 null 回收线程。runWorker 的 finally 调 processWorkerExit() 移除 worker 并按状态补线程；任务异常存入 thrown 交 afterExecute。shutdown() 用 tryLock 只中断空闲线程，shutdownNow() 则可能打断任务。

## 要点

- Worker 是 ThreadPoolExecutor 内部类，封装线程池中的线程，持有 thread 与 firstTask 两个关键属性。
- Worker 继承 AQS 实现不可重入独占锁，构造时 state 置 -1，防止新线程在运行前被中断。
- 加锁表示 worker 正在执行任务：shutdown() 经 interruptIdleWorkers 的 tryLock 只中断空闲线程。
- run() 委托 ThreadPoolExecutor.runWorker()，循环执行 firstTask 或经 getTask() 从队列取任务。
- getTask() 按 timed 用 poll(keepAliveTime) 或 take()，超时返回 null 触发非核心线程回收。
- processWorkerExit() 移除 worker、统计完成数，并按线程池状态决定是否 addWorker 补充线程。
- 任务异常被 catch 存入 thrown，在 finally 交 afterExecute 处理，可覆写该方法处理内部异常。

## 提炼

> 一句话定位：逐行拆解线程池内部类 Worker 的锁、属性与执行流程

### 分节提炼

- **01 简介**：Worker 是 ThreadPoolExecutor 的内部类，相当于线程池中存储的线程。；Worker 不是单纯线程，其属性中持有一个线程，并承担数据统计与任务执行相关功能。
- **02 类图**：本节以类图形式呈现 Worker 类的继承与实现结构。；正文未附文字说明，具体字段与继承关系见后续源码章节。
- **03 Worker对象解释**：线程池线程被封装为 Worker 存入 workers 集合；firstTask 保存任务，thread 由 ThreadFactory 创建。；Worker 继承 AQS 实现不可重入独占锁，构造时 state=-1，新线程处理任务前不允许被中断。；lock() 加锁表示正在执行任务；空闲线程可被 interruptIdleWorkers 用 tryLock 判断并中断。；用 AQS 而非 ReentrantLock，是为实现不可重入以反映线程当前执行状态。
- **04 主要代码**：Worker 源码很短，主要成员为 thread、firstTask、completedTasks，及构造方法与 run()。；构造器先 setState(-1) 抑制中断，再记录 firstTask，并用 getThreadFactory().newThread(this) 创建线程。；run() 直接委托 ThreadPoolExecutor.runWorker(this)，真正逻辑在外层类中。；worker 本质是 Runnable：需构造 Thread 并 start()，线程才会进入 runWorker 循环取任务执行。
- **05 源码分析**：runWorker() 先执行 firstTask，再 while 循环经 getTask() 取任务；每任务执行前 w.lock()、执行后 w.unlock() 并 completedTasks++。；getTask() 按 timed 选 poll(keepAliveTime) 或 take()，超时返回 null 使非核心线程退出循环被回收。；getTask() 返回 null 后，runWorker 的 finally 调 processWorkerExit()：减计数、移除 worker、统计完成任务，必要时 addWorker 补线程。；任务异常被 catch 存入 thrown，finally 交 afterExecute；可覆写 afterExecute 处理线程池内部异常。
- **06 生命周期**：本节以流程图展示 Worker 执行任务的整体生命周期模型。；正文仅给出图示与相关文章链接，无额外文字说明。

### 带走三句话

1. Worker 核心：持有 thread 与 firstTask，借 AQS 不可重入锁反映“是否正在执行任务”。
2. runWorker 的 while + getTask() 决定线程生死：取不到任务返回 null，线程退出并被回收。
3. shutdown() 靠 tryLock 只中断空闲线程；需要优雅停机就不要用 shutdownNow()。

### 知识卡

跨全文挖掘 11 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
