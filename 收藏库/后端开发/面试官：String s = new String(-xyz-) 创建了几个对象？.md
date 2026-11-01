---
title: "面试官：String s = new String(\"xyz\") 创建了几个对象？"
url: "https://mp.weixin.qq.com/s?__biz=Mzg3MzY2OTcyMQ==&mid=2247488183&idx=1&sn=152b8cab201c4c99eab68703d9a798f5"
source: "微信公众平台 · Java后端编程"
published: "2023-09-08"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发, 面试]
lang: zh
status: ok
阅读页: "阅读页/后端开发/面试官：String s = new String(-xyz-) 创建了几个对象？.html"
---

# 面试官：String s = new String("xyz") 创建了几个对象？

> 一句话定位：javap 反编译证明 new String("xyz") 创建 1~2 个对象

## 摘要

文章用 javap 反编译解析 String s = new String("xyz") 创建几个对象。方法区在 JDK1.7 后把字符串常量池移到堆，1.8 后永久代改为元空间、常量池仍在堆。反编译显示 ldc 在常量池创建 "xyz" 对象，new 在堆创建 String 对象，共 2 个对象；说 3 个是把引用 s 也算作对象。再次 new String("xyz") 时第二次 ldc 索引同为 #2，常量池不重复创建。结论：引用不算对象，xyz 不存在创建 2 个，存在则 1 个。

## 要点

- 常量池随 JDK 演变：1.7 前在方法区（非堆），1.7 起移入堆，1.8 起永久代被元空间取代、常量池仍在堆。
- 全文以 JDK1.8 为基础讨论，避免版本差异造成结论分歧。
- javac 编译后 javap -c 反编译，ldc 在常量池创建 "xyz" 对象并压栈。
- new 在堆中创建 String 对象，随后 invokespecial 执行构造函数。
- String s = new String("xyz") 创建 2 个对象；说 3 个是把引用 s 也算作对象。
- 再次 new String("xyz") 时第二次 ldc 的索引仍为 #2，常量池不重复创建 "xyz"。
- 最终答案：引用不算对象，xyz 不存在创建 2 个、存在则 1 个。
- 文中罗列 nop、ldc、new、invokevirtual 等常见字节码指令助记符。

## 提炼

> 一句话定位：javap 反编译证明 new String("xyz") 创建 1~2 个对象

### 分节提炼

- **01 nop 指令**：nop 是什么都不做的空指令。；指令助记符用于理解 javap -c 反编译输出。
- **02 aconst_null 指令**：aconst_null 把 null 推送至栈顶。；属于本文罗列的常见指令助记符。
- **03 iload 指令**：iload 把指定 int 型局部变量压栈。；相对的 istore 把栈顶 int 值存入局部变量。
- **04 dup 指令**：dup 复制栈顶数值并压入栈顶。；属于本文罗列的常见指令助记符。
- **05 invokevirtual 指令**：invokevirtual 调用实例方法。；属于本文罗列的常见指令助记符。
- **06 invokestatic 指令**：invokestatic 调用静态方法。；属于本文罗列的常见指令助记符。
- **07 invokeinterface 指令**：invokeinterface 调用接口方法。；属于本文罗列的常见指令助记符。
- **08 invokedynamic 指令**：invokedynamic 调用动态链接方法。；属于本文罗列的常见指令助记符。
- **09 new 指令**：new 创建对象并把引用值压栈。；示例中 new 创建的是堆里的 String 对象。
- **10 xyz 不存在且引用算对象**：xyz 不存在时 ldc 与 new 各建 1 个对象，共 2 个。；把引用 s 也计入才变成 3 个。
- **11 xyz 不存在且引用不算**：xyz 不存在且引用不算时，创建 2 个对象。；与上一条区别只在是否计入引用 s。
- **12 xyz 存在且引用算对象**：xyz 已存在时常量池不新建，仅 new 建 1 个。；再计入引用 s 才记为 2 个。
- **13 xyz 存在且引用不算**：作者认为引用不算对象。；最终答案应为 1 个或 2 个，取决于 xyz 是否已存在。

### 带走三句话

1. 判断创建几个对象，关键是看 ldc 与 new 各自创建了什么，而不是数引用。
2. 用 javap -c 反编译看字节码，比背结论更可靠。
3. 面试答 2 个（xyz 不存在）或 1 个（xyz 已存在），并说明引用不算对象。

### 知识卡

跨全文挖掘 6 张知识卡（代码模式 / 数据 / 易错点 / 结论），带原文锚点，完整内容见阅读页提炼层。
