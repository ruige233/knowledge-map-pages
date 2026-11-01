---
title: "Java反射和new效率对比，你知道差距有多大吗？"
url: "https://mp.weixin.qq.com/s?__biz=MzIyNDU2ODA4OQ==&mid=2247507805&idx=1&sn=816a288511acccaa6bf9ab4a1f72f55c"
source: "微信公众平台"
published: "2023-09-16"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发, Java]
lang: zh
status: ok
阅读页: "阅读页/后端开发/Java反射和new效率对比，你知道差距有多大吗？.html"
---

# Java反射和new效率对比，你知道差距有多大吗？

> 一句话定位：实测对比 Java new 与反射创建对象的性能差距与成因

## 摘要

文章通过实测对比 Java 中 new 与反射创建对象的性能差距。作者用 for 循环分别创建 1 亿个 ReflectDemo 对象，new 与反射（Class.newInstance）耗时相差很多倍。原因在于反射是动态解析：Method#invoke 需要对参数封装与解封、检查方法可见性、校验参数，反射方法难以内联，JIT 也无法优化。随后介绍反射的典型使用场景：Spring 通过反射实例化对象放入 IoC 容器、JDBC 用 Class.forName 加载驱动、逆向反编译，以及借助反射在 int 泛型 ArrayList 中存入 String。最后总结 new 与反射的区别：new 无法访问私有属性而反射可通过 setAccessible 访问，new 必须知道类名而反射无需知道类型。

## 要点

- new 与反射创建对象效率相差很多倍，1 亿次循环实测差距明显。
- 反射慢源于动态解析：参数封解、可见性检查、参数校验、难内联、JIT 无法优化。
- 反射创建对象的三种方式：类字面量、Class.forName、实例 getClass。
- Spring 借助反射实例化对象并放入 IoC 容器。
- JDBC 通过 Class.forName 加载数据库驱动。
- 反射可绕过泛型检查，在 int 型 ArrayList 中存入 String。
- new 无法访问私有属性，反射可 setAccessible 访问。
- new 必须知道类名，反射创建对象无需知道类型。

## 提炼

> 一句话定位：实测对比 Java new 与反射创建对象的性能差距与成因

### 分节提炼

- **01 基础概念**：对比 new 与反射两种创建对象方式的效率与适用时机；给出反射创建对象的三种 Class 获取方式代码
- **02 效率对比**：用 1 亿次循环实测 new 与 newInstance 耗时，差距很多倍；解析反射慢的五个原因：参数封解、可见性检查、参数校验、难内联、JIT 无法优化；Java 经 javac 编译为 class，JIT 可优化字节码，反射动态解析难获优化
- **03 使用场景**：反射场景：Spring IoC、JDBC 驱动加载、反编译、泛型绕过；区别：new 无法访问私有属性、必须知道类名；反射可 setAccessible、无需知道类型

### 带走三句话

1. 类型固定且需大量创建时优先用 new，反射存在明显性能开销。
2. 反射的代价来自参数封解、可见性与参数校验、难以内联及 JIT 无法优化。
3. 反射的价值在运行时动态性：IoC 实例化、JDBC 驱动加载、反编译与泛型绕过都依赖它。

### 知识卡

跨全文挖掘 5 张知识卡（代码模式 / 数据 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
