---
title: "一个 List.of 引发的“血案”"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247535686&idx=1&sn=46e4b57fe63940f65ce0eb0298212c97"
source: "微信公众平台"
published: "2023-11-09"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发]
lang: zh
status: ok
阅读页: "阅读页/后端开发/一个 List.of 引发的“血案”.html"
---

# 一个 List.of 引发的“血案”

> 一句话定位：用 List.of() 返回空列表，经 Node 泛化调用被序列化成带 tag 字段的对象导致解析失败

## 摘要

一次后端发布把接口默认返回从 null 改为 List.of()，前端 Node 泛化调用 HSF 接口后，结果从预期的空数组变成带 tag 字段的对象，导致解析失败。作者顺源码排查：List.of() 底层是 ImmutableCollections.emptyList()，实际返回内部类 ListN 而非 ArrayList；ListN 的 readObject 直接抛异常防御反序列化，writeReplace 把自身替换为序列化代理 CollSer，构造传入 tag=1 与空数组，array 被 transient 修饰忽略，于是 Node 解析出带 tag=1 的对象。修复只需换回 Lists.newArrayList()。反思：用新特性前先搞清底层实现。

## 要点

- 后端把接口默认返回从 null 改成 List.of()，前端 Node 泛化调用后解析出带 tag 字段的对象而非空数组，导致字段解析失败。
- List.of() 是 JDK9+ 特性，返回不可修改的空列表，底层走 ImmutableCollections.emptyList()，实际类型是内部类 ListN。
- ListN 继承 AbstractImmutableList 并实现 List 与 Serializable，EMPTY_LIST 静态变量初始化为空 ListN 对象。
- ListN 的 readObject 直接抛 InvalidObjectException 防御直接反序列化；writeReplace 将自身替换为序列化代理 CollSer。
- 序列化时 new CollSer(CollSer.IMM_LIST, elements) 传入 tag=1 和空数组，array 被 transient 修饰忽略，Node 侧解析出带 tag=1 的对象。
- 修复简单：把 List.of() 换成 Lists.newArrayList() 即可恢复预期的空数组。
- HSF 控制台用 hessian 序列化能正确解析，怀疑与 Node 泛化调用的序列化类型不同，待后续确认。
- 反思：使用新技术或新特性前，必须先搞清楚内部实现细节，否则容易踩坑。

## 提炼

> 一句话定位：用 List.of() 返回空列表，经 Node 泛化调用被序列化成带 tag 字段的对象导致解析失败

### 分节提炼

- **01 List.of 源码**：List.of() 是 JDK9 之后特性，返回不可修改列表，官方注释明确返回空 List。；底层实现是 ImmutableCollections.emptyList()，ImmutableCollections 是不可变集合容器类。
- **02 emptyList 与 ListN**：emptyList() 强转后实际返回 ListN 类型对象，而非 ArrayList。；ListN 继承 AbstractImmutableList，实现 List 与 RandomAccess。；EMPTY_LIST 静态变量初始化为空 ListN 对象，是排查的关键线索之一。
- **03 Node HSF 泛化调用**：陆游平台通过 Node 泛化调用 HSF 接口，默认只解析 List、Map 等基础 Java 类型。；ListN 这类特殊类型在序列化/反序列化时走了不同逻辑，导致异常结构。
- **04 ListN 序列化代理**：readObject 直接抛 InvalidObjectException，是保证不可变性的防御措施。；writeReplace 把 ListN 替换成内部序列化代理 CollSer，传入 tag=1 与空数组。；CollSer 的 array 字段被 transient 修饰忽略，tag=1 就是前端对象里 tag 的来源。
- **05 结论与反思**：解决办法是把 List.of() 替换成 Lists.newArrayList()。；HSF 控制台用 hessian 序列化可正确解析，怀疑与 Node 序列化类型不同。；反思：用新技术前先搞清楚内部实现细节。

### 带走三句话

1. List.of() 返回不可变内部类 ListN，跨语言泛化调用时会被序列化成带 tag 的对象，别当普通 List 用。
2. 排查诡异序列化问题时，先看类的 writeReplace/readObject 与 transient 字段，答案往往在源码里。
3. 上线新特性前先读懂底层实现，遇到异常结构优先对比序列化协议差异。

### 知识卡

跨全文挖掘 6 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
