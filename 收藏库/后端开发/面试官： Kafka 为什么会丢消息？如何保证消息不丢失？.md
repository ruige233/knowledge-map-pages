---
title: "面试官： Kafka 为什么会丢消息？如何保证消息不丢失？"
url: "https://mp.weixin.qq.com/s?__biz=MzU2OTMyMTAxNA==&mid=2247526307&idx=1&sn=f5206540f3da5692f3e5da4dc16146c2"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发, Kafka, 面试]
lang: zh
status: ok
阅读页: "阅读页/后端开发/面试官： Kafka 为什么会丢消息？如何保证消息不丢失？.html"
---

# 面试官： Kafka 为什么会丢消息？如何保证消息不丢失？

> 一句话定位：Kafka 丢消息三问：如何感知、哪些环节、如何保证不丢失

## 摘要

文章围绕 Kafka 消息丢失，按“如何感知、哪些环节、如何保证”三步展开。感知层面：靠 PM/运营反馈、监控报警，以及 GetOffsetShell、kafka-consumer-groups 等命令检索消费位置。丢失环节：生产端网络波动、无 ack 无回调；Broker 写 PageCache 后掉电、副本同步不一致；消费端自动提交 offset、心跳超时触发 Rebalance。保障措施：生产端配置 retries、acks=all、发送回调；Broker 用蓄电池缓存、副本数≥3、min.insync.replicas>1、关闭 unclean 选主并启用 Epoch；消费端升级客户端至 0.10.2+、关闭自动提交改手动 ack、提高消费速度。全文落脚于“MQ 可靠消息投递”综合方案。

## 要点

- 引入 MQ 是为系统解耦与流量控制，但会带来数据一致性问题，需保证消息不丢失。
- 通过他人反馈、监控报警与消费位置检索（GetOffsetShell、consumer-groups 命令）感知和定位丢消息。
- 生产端丢消息源于网络波动与不恰当配置（无 ack、无回调），需 retries、acks=all、发送回调。
- Broker 写 PageCache 后机器掉电会丢数据，需蓄电池缓存并理解日志刷盘机制。
- Broker 可靠性依赖多副本：replication.factor≥3、min.insync.replicas>1、ISR 与 Epoch 机制。
- 消费端自动提交 offset 与心跳超时 Rebalance 是主要丢消息场景，需手动 ack 并升级客户端 0.10.2+。
- 确保不丢失需熟悉全链路、监控 Kafka 集群并落地“MQ 可靠消息投递”方案。

## 提炼

> 一句话定位：Kafka 丢消息三问：如何感知、哪些环节、如何保证不丢失

### 分节提炼

- **01 感知与定位**：他人反馈与监控报警是感知消息丢失的主要途径；可用 GetOffsetShell 查消息量、consumer-groups 查 offset/LAG 定位；舆情采集案例：PM 下发采集指令并用 ES 近实时查询复核数据
- **02 三个丢消息环节**：生产端 send() 是缓存后批量发送，网络波动需 retries，无 ack/回调需补配置；acks=0/1/all 可靠性递增：0 易丢、1 丢 leader、all 最可靠；Broker 写 PageCache 后 Broker 宕机不丢、机器掉电会丢，需蓄电池缓存；消费端自动提交 offset、心跳超时 Rebalance 是主要丢消息原因
- **03 不丢失保障措施**：生产端 retries、acks=all、发送回调三件套；Broker 侧蓄电池缓存、0.11.x+ Epoch、多副本与 min.insync.replicas>1；消费端升级 0.10.2+、手动 ack、提高消费速度

### 带走三句话

1. 丢消息没有单一答案：先按生产端、Broker、消费端三段排查，再逐段对症配置。
2. 最省心组合是生产端 retries+acks=all+回调，Broker 多副本+Epoch，消费端手动 ack。
3. 监控与检索命令（offset、LAG）是定位丢消息的起点，配置前先确认丢在哪一段。

### 知识卡

跨全文挖掘 10 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
