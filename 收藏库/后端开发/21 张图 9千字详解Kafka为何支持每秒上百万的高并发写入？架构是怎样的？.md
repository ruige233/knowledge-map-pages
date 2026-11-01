---
title: "21 张图 9千字详解Kafka为何支持每秒上百万的高并发写入？架构是怎样的？"
url: "https://mp.weixin.qq.com/s?__biz=MzkzMDI1NjcyOQ==&mid=2247506199&idx=1&sn=1be8e2b8c134d3006bfe891ecdbfd4f4"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发, Kafka, 高并发]
lang: zh
status: ok
阅读页: "阅读页/后端开发/21 张图 9千字详解Kafka为何支持每秒上百万的高并发写入？架构是怎样的？.html"
---

# 21 张图 9千字详解Kafka为何支持每秒上百万的高并发写入？架构是怎样的？

> 一句话定位：从 I/O 模型到无锁 offset，拆解 Kafka 百万级吞吐的实现路径

## 摘要

本文逐层拆解 Kafka 单机高吞吐的底层设计：网络层用 Reactor + NIO 多路复用承接海量连接；存储层靠 Topic→Partition→Segment 的分段日志与稀疏索引定位消息；性能层通过顺序写盘、零拷贝（sendfile/mmap）与 PageCache 把磁盘开销压到最低；再叠加 Producer 端批量压缩和无锁 offset 管理，最终把「每秒百万级写入」落成一套可复制的工程组合。适合面试与架构选型时按「网络→存储→性能→可靠性」主线串讲。

## 要点

- Reactor + NIO 三级角色（Acceptor/Processor/KafkaRequestHandler）把连接接入与业务处理解耦
- 零拷贝 sendfile + mmap 绕开用户态复制，减少数据拷贝次数
- Topic 是逻辑队列，Partition 是并发与负载均衡的物理单元
- Segment 分段日志 + .index/.timeindex 稀疏索引支撑海量消息检索
- 顺序写盘 + PageCache 让磁盘读写接近内存
- 压缩在 Producer 端批量进行、Broker 原样保存、Consumer 端解压
- offset 用顺序写 + mmap + 零拷贝实现无锁轻量管理
- 压缩算法按压缩比与吞吐权衡：gzip/snappy/lz4/zstd

## 提炼

> 一句话定位：从 I/O 模型到无锁 offset，拆解 Kafka 百万级吞吐的实现路径

### 分节提炼

- **01 Reactor 网络模型**：Reactor 通过一个多路复用器监听多个通道事件，避免每连接一线程；SocketServer 管理连接，Acceptor 只做接入分发，Processor 做读写，RequestChannel 缓冲请求；KafkaRequestHandler 从队列取请求、调 KafkaApis 处理并回写响应
- **02 零拷贝**：sendfile 把磁盘数据直接发到网络套接字，省去用户态拷贝；mmap 把日志/索引映射到内存，读写像访问内存一样直接；传统 I/O 要 4 次 copy，零拷贝减少 CPU 与内存带宽消耗
- **03 分区与负载均衡**：每增加一个 Partition 就增加一个消费并发；分区策略：轮询/随机/按 Key 保序/基于地理位置；轮询负载最均衡，是默认策略
- **04 Segment 与稀疏索引**：日志过大就切 Segment，文件名为上一条消息的 offset；.index 按 offset、.timeindex 按时间做稀疏索引；稀疏索引每隔 log.index.interval.bytes 才记一条，避免撑爆内存
- **05 顺序写盘**：顺序 I/O 比随机 I/O 快 3-4 个数量级；磁盘 I/O 由寻道、旋转、传输三步构成，顺序写省去寻道旋转；Partition 内消息追加写，天然适合顺序写盘
- **06 PageCache**：写操作只写 PageCache 并标记 Dirty，由 OS 决定刷盘时机；读操作先查 PageCache，缺页才走磁盘调度；生产消费速率接近时，几乎全程在 page cache 完成
- **07 压缩与批处理**：Producer 端压缩、Broker 端原样保存、Consumer 端解压；压缩算法：GZIP/Snappy/LZ4/Zstandard（2.1.0 起）；批处理按 batch.size 与 linger.ms 触发发送，降低网络开销
- **08 无锁 offset**：offset 表示消息在分区的位置，提交位移表征消费进度；无锁实现：顺序写 + mmap + 零拷贝 + 批量处理；offset 不可变，消息删除/过期也不会重用
- **09 总结**：吞吐 = Reactor 网络模型 + 顺序写 + mmap + 零拷贝 + 压缩批处理叠加；Kafka 把磁盘、内存、网络的优化点组合成体系

### 带走三句话

1. Kafka 百万级吞吐是「顺序写 + 零拷贝 + PageCache + 批量压缩」四件套叠加，不是单点优化。
2. 海量消息检索靠 Segment 分段 + 稀疏索引：二分定位、顺序扫描，绝不用全量哈希索引。
3. 压缩与解压分工明确：Producer 压、Broker 存、Consumer 解，算法按压缩比与吞吐权衡。

### 知识卡

跨全文挖掘 6 张知识卡（数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
