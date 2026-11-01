---
title: "这些年背过的面试题——Netty篇"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247537942&idx=1&sn=e51e01a6e984845c288edeea48f3affd"
source: "微信公众平台"
published: "2024-06-14"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发, Netty, 面试]
lang: zh
status: ok
阅读页: "阅读页/后端开发/这些年背过的面试题——Netty篇.html"
---

# 这些年背过的面试题——Netty篇

> 一句话定位：Netty 面试高频点全景：架构分层、IO 模型、内存管理与高性能数据结构

## 摘要

本文是 Netty 面试题合集，梳理架构与高频考点。整体结构分 Core 核心层、协议支持层、传输服务层；逻辑架构由网络通信、事件调度、服务编排三层组成。随后讲五种 IO 模型、Reactor 线程模型、拆包粘包解决方案、自定义协议编解码和 WriteAndFlush 出站流程。内存管理覆盖堆外内存、ByteBuf、jemalloc 与零拷贝；高性能数据结构覆盖 FastThreadLocal、时间轮和 select/poll/epoll 区别。

## 要点

- Netty 整体分三层：Core 核心层、Protocol Support 协议支持层、Transport Service 传输服务层。
- 逻辑架构分网络通信层、事件调度层、服务编排层，核心组件有 Bootstrap、EventLoopGroup、ChannelPipeline 等。
- EventLoopGroup 本质是线程池，通过不同配置支持单线程、多线程、主从多线程三种 Reactor 模型。
- 五种 IO 模型：BIO、NIO、多路复用、信号驱动、异步 IO；Netty 基于 NIO 的 Selector 实现。
- TCP 拆包粘包源于面向流无边界，解决方案为固定长度、特定分隔符、消息长度+内容三种。
- ByteBuf 读写索引分离解决 JDK ByteBuffer 的翻转切换问题，并通过引用计数与对象池管理内存。
- jemalloc 按 arena/bin/chunk/run/region 分级管理，tcache 线程私有缓存优先分配避免锁竞争。
- Netty 零拷贝体现在堆外内存、CompositeByteBuf、wrappedBuffer、slice、FileRegion 等用户态优化。

## 提炼

> 一句话定位：Netty 面试高频点全景：架构分层、IO 模型、内存管理与高性能数据结构

### 分节提炼

- **01 整体结构**：Core 核心层提供事件模型、通用 API 与支持零拷贝的 ByteBuf；协议支持层覆盖 HTTP、Protobuf、WebSocket 等编解码实现；传输服务层抽象封装 TCP/UDP 传输，让用户聚焦业务逻辑
- **02 逻辑架构**：网络通信层组件：Bootstrap、ServerBootStrap、Channel；事件调度层：EventLoopGroup、EventLoop，支持三种 Reactor 线程模型；服务编排层 ChannelPipeline 双向链表串联 ChannelHandler
- **03 五种IO模型**：BIO 一个请求对应一个线程，线程切换开销大；NIO 调用后立即返回靠轮询取结果，系统调用带来上下文切换开销；多路复用一个线程处理多个 Socket，select 只负责等、recvfrom 负责拷贝；异步 IO 由内核拷贝数据后通过回调函数处理
- **04 Reactor多线程模型**：Netty 基于 NIO 的 Selector，epoll 模式单线程负责轮询；事件分发器有 Reactor（同步 IO）和 Proactor（异步 IO）两种；Reactor 实现简单适合耗时短场景，Proactor 性能高但实现复杂
- **05 拆包粘包**：TCP 面向流、没有数据包界限，服务端一次读到的大小不确定；MSS+TCP 首部+IP 首部大于 MTU 时数据包被拆分；Nagle 批量发送，Netty 默认禁用以最小化传输延迟；确定边界方案：消息长度固定、特定分隔符、消息长度+消息内容
- **06 自定义协议**：编码器：MessageToByteEncoder、MessageToMessageEncoder；解码器：ByteToMessageDecoder、ReplayingDecoder 等；一次解码器解决拆包粘包，二次解码器做对象模型转换；通过读取 dataLength 判断 ByteBuf 是否已有完整报文
- **07 WriteAndFlush**：writeAndFlush 是出站操作，从 Tail 节点向 Head 节点传播；write 只把数据写入 ChannelOutboundBuffer 缓存（单向链表）；flush 才最终把数据写入 Socket 缓冲区
- **08 堆外内存**：堆外内存不受 JVM 管理，可降低 GC 影响、减少一次内存拷贝、便于共享；DirectByteBuffer 由 Cleaner 在 GC 时回收堆外内存；长时间无 GC 时堆外内存不释放，需 -XX:MaxDirectMemorySize 限制上限
- **09 ByteBuf**：JDK ByteBuffer 定长难扩容，读写共用 position 需切换；ByteBuf 分废弃、可读、可写、可扩容四区，读写索引分离；引用计数为 0 时可放入对象池复用，避免重复创建；Netty 抽样检测不可达且引用计数大于 0 的对象，日志 LEAK 关键字定位泄漏
- **10 内存分配算法**：动态内存分配：首次适应、循环首次适应、最佳适应；伙伴算法按 2 的次幂划分，外部碎片少、内部碎片多；Slab 算法针对小内存用内存池方案解决内部碎片
- **11 jemalloc架构**：内存由多个 arena 管理，线程 round-robin 选择 arena；arena 内 bin/chunk/run/region 分级，run 是实际分配单位；tcache 是线程私有缓存，优先分配避免锁竞争；Small/Large 走 tcache 或 chunk 分配，Huge 直接 mmap 分配
- **12 零拷贝**：传统 read+send 有四次数据拷贝和四次上下文切换；Linux sendfile() 与 transferTo() 实现零拷贝；Netty 零拷贝：堆外内存、CompositeByteBuf、slice、FileRegion
- **13 FastThreadLocal**：JDK 在 Thread 中维护 ThreadLocalMap 避免加锁；key 被 GC 后 value 强引用仍占内存，需主动 remove() 清理；FastThreadLocal 用 Object 数组按 index 取值 O(1)；FastThreadLocalRunnable 任务执行完自动 removeAll()
- **14 时间轮**：JDK 定时器：Timer、DelayedQueue 等；Timer 基于系统绝对时间且异常会终止线程；DelayedQueue 常用于重试机制；时间轮按到期时间取余分布到 slot，新增和执行近似 O(1)；空推进问题由 Kafka 用 DelayQueue 阻塞推进 + 层级时间轮解决
- **15 定时形式一**：定时任务三种形式之一：按固定周期定时执行；配合 Schedule/Cancel/Run 三个关键方法使用
- **16 定时形式二**：定时任务三种形式之二：延迟一定时间后执行；配合 Schedule/Cancel/Run 三个关键方法使用
- **17 定时形式三**：定时任务三种形式之三：指定某个时刻执行；配合 Schedule/Cancel/Run 三个关键方法使用
- **18 select/poll/epoll**：select/poll 遍历查询每个 fd 状态，无就绪则挂起，唤醒后再次遍历；epoll 通过 epoll_ctl 注册 fd，就绪以 callback 回调方式通知；Epoll 空轮询漏洞：事件列表为空仍唤醒 NIO 线程致 CPU 100%

### 带走三句话

1. Netty 先抓核心组件：Bootstrap 引导、EventLoopGroup、ChannelPipeline。
2. 写自定义协议要解决拆包粘包：定长度、分隔符或消息长度+内容，并用读写索引分离的 ByteBuf 承载数据。
3. 内存与性能记住三件事：堆外内存注意回收、零拷贝减少搬运、时间轮与 FastThreadLocal 降低查找开销。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
