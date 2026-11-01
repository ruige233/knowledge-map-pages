---
title: "在 IDEA 中的各种调试技巧，轻松定位 Bug（超级全面）"
url: "https://mp.weixin.qq.com/s?__biz=MzkwOTAyMTY2NA==&mid=2247493550&idx=2&sn=55930f647be50a54dcf3edd2c3bd1b5d"
source: "微信公众平台"
published: "2023-09-22"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发]
lang: zh
status: ok
阅读页: "阅读页/后端开发/在 IDEA 中的各种调试技巧，轻松定位 Bug（超级全面）.html"
---

# 在 IDEA 中的各种调试技巧，轻松定位 Bug（超级全面）

> 一句话定位：系统梳理 IDEA 各调试按钮、快捷键、断点、多线程与回退技巧

## 摘要

文章系统讲解 IntelliJ IDEA 的 Debug 调试用法：开篇介绍界面八个区域（启动方式、断点、Debug 窗口、调试按钮、方法调用栈、Variables、Watches）；随后说明两组调试按钮及快捷键（步过、步入、强制步入、步出、计算表达式、恢复与停止等）；变量查看有四种途径：行内显示、悬停、Variables、Watches；再介绍计算表达式与改值、智能步入、条件与异常断点、多线程改为 Thread、断点回退 Drop Frame、Force Return 中断调试，附 JRebel 激活。

## 要点

- Debug 界面分八个区域：启动、断点、Debug 窗口、调试按钮、服务按钮、调用栈、Variables、Watches
- 第一组 8 个按钮负责逐行执行：步过 F8、步入 F7、强制步入、步出、运行到光标、计算表达式等
- 第二组 7 个按钮负责运行控制：重新运行、更新、恢复、暂停、停止、查看与静音断点
- 查看变量有四种途径：参数行后值、光标悬停、Variables 区、Watches 监视
- 断点可设置条件、Pass count 与异常断点，并支持把断点信息输出到控制台
- 多线程调试要把断点 Suspend 改为 Thread，避免默认 ALL 阻塞其它线程
- 断点回退 Drop Frame 只回到上一方法调用处，已改变的状态无法回滚
- 想提前结束调试可用 Force Return 强制返回；JRebel 可热更新减少重启

## 提炼

> 一句话定位：系统梳理 IDEA 各调试按钮、快捷键、断点、多线程与回退技巧

### 分节提炼

- **01 Debug 界面概览**：Debug 界面可分为启动、断点、Debug 窗口、调试按钮、服务按钮、调用栈、Variables、Watches 八个区域；勾选 Show debug window on breakpoint 可让断点命中时自动激活 Debug 窗口；勾选 Show All Frames 可只显示本线程调用链，隐藏其它类库方法
- **02 基本用法与快捷键**：调试功能主要对应两组按钮，悬停按钮可查看快捷键；Run 菜单里有相同的调试功能
- **03 第一组调试按钮**：Step Over 步过不进方法；Step Into 只进自定义方法，不进官方类库；Force Step Into 可进任何方法，适合看底层源码；Run to Cursor 运行到光标行，无需打断点
- **04 第二组调试按钮**：Resume Program 跳到下一断点，无断点则跑完整流程；Mute Breakpoints 让所有断点变灰失效，单个断点可右键取消 Enabled；On frame deactivation 建议设为 Do nothing，避免频繁切换窗口消耗资源
- **05 变量查看**：四种查看方式：参数行后值、光标悬停详情、Variables 区、Watches 监视；Watches 可 New Watch 新建，或从 Variables 拖入变量
- **06 行内显示变量值**：参数行后直接显示变量值，悬停可展开详情
- **07 计算表达式**：选中表达式按 Alt+F8 可计算值，也能算某方法的返回值；在计算框里可改变量值，方便测试不同取值
- **08 智能步入**：Smart Step Into（Shift+F7）会列出当前行所有方法供选择；只有一个方法时直接进入，类似 Force Step Into
- **09 断点条件设置**：右键断点或 View Breakpoints 设置 Condition，满足条件才暂停；勾选 Log message to console 与 Evaluate and log 可把断点信息输出到控制台；添加 Java Exception Breakpoints 可自动定位到异常行
- **10 多线程调试**：Debug 默认 Suspend=ALL 会阻塞其它线程，改为 Thread 并 Make Default；Frames 下拉列表里可切换当前调试线程
- **11 回退断点**：方法调用栈最上面是当前断点所在方法，往下是被逐层调用的方法
- **12 断点回退操作**：Drop Frame 只能回退到上一个方法调用开始处，不能逐行回退；可在调用栈方法上右键 Drop Frame，再按 F9 重新进入断点
- **13 中断 Debug**：除关闭服务外没有直接中断请求的方式，可用 Force Return 强制返回；Force Return 按返回类型返回结果或 new 实例，跳过后续流程
- **14 JRebel 激活**：JRebel 可热更新 Java 代码与 xml 配置，改动多时偶发失效需重启；文中介绍通过 my.jrebel.com 登录获取永久激活码，稳定性不定

### 带走三句话

1. 先记住两组调试按钮与快捷键：步过/步入控制逐行执行，恢复/停止/静音断点控制整体运行。
2. 善用条件断点、异常断点和 Thread 模式，在遍历大集合、定位异常、多线程场景能少走很多弯路。
3. 回退断点只回退流程不回滚数据状态；想提前终止用 Force Return 强制返回，不必重启服务。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
