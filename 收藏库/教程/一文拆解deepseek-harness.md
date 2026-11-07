---
title: "一文拆解DeepSeek Harness"
url: "https://mp.weixin.qq.com/s/Lksj3TFBRNt4jAkdbn4Jbg"
source: "微信公众平台 · 腾讯云开发者"
published: "2026-11-05"
archived: "2026-11-05"
category: "教程"
tags: [Agent, Agent 编排, 上下文工程, 多模型路由, Harness]
lang: zh
status: ok
阅读页: "阅读页/教程/一文拆解deepseek-harness.html"
---

# 一文拆解DeepSeek Harness

> 一句话定位：以 DeepSeek《Super-agent 设计探索》为底本，拆解 sessionless 的 Super-agent 系统架构——把记忆、调度、审批从人身上搬进系统。

## 摘要

本文是作者对 DeepSeek 10 月 31 日《Super-agent 设计探索》一文的深度解读，回答"Super-agent 到底解决了什么问题、靠什么架构实现"。作者从"一个人只能同时跟进 3 到 5 个活跃 Session"这个被当成产品体验的观察切入，指出真正瓶颈是记忆、调度、审批、恢复都被交给了人，因此目标应转为"最大化 Agent 产出、最小化人的注意力"。全文沿两条线展开：一是系统架构（Router 控制面与 Worker 数据面分离、上下文边界与链式召回、事件到工作的数据映射、计算与状态分离的 Agent as a Service、端云协同、事件驱动订阅直通、全双工多信道、Watchdog 巡检）；二是作者对系统边界之外的三点设想——身份（生态）、感知（Trigger 即传感器）、界面（Agent 能自己长出超级 App）。结论：单个 Agent 的智能上限很快会被拉平，真正值钱的是"它能以什么身份进哪些系统"——真正的 Super 不在于多聪明，而在于能连上多少东西。

## 要点

- "3–5 个活跃 Session" 瓶颈的本质：不是人的注意力有限，而是记忆、调度、审批、恢复被交给人承担；"Session 应当是系统的实现细节，而不应是用户的认知负担"。
- Router 可行的前提不是聪明，而是上下文有边界：群聊、GitHub、项目文档天然自带工作区 Context；边界内 Fast Path 用代码规则写死、Slow Path 才交 LLM 决策。
- 链式召回：把上下文窗口当 Cache，所有对象有 ID、先只送摘要和 ID，需要细节再顺 ID 逐跳取——"浪费上下文窗口，就是浪费模型的智能"。
- 三大分离：调度与执行分离（Router 控制面 vs Worker 数据面）；计算与状态分离（Agent as a Service，执行 Loop 无状态、可替换）；执行与环境分离（端云协同，Sandbox 从"Agent 的容器"变成"Agent 的工具"）。
- 事件驱动：Trigger 统一投喂 + Fast/Slow Path 过滤 + 订阅直通（订阅过的 Thread 直接送 Worker，Router 不当瓶颈）；全双工 start()/stop() 解耦前台对话与后台执行；IM 场景单次回复不超过 100 词。
- 作者三点设想：身份（生态 = 有多少系统愿意认这个身份，最后值钱的是身份而非模型）、感知（Trigger 本质是感知力，连接器要像传感器订阅状态变化）、界面（Agent 应能给自己长出持续运行、双向调用、边界明确的超级 App）。

## 关联

- 与《为什么AI会“选择性失忆”：搞懂Transformer架构》同属"教程/原理解读"；上下文窗口与记忆管理主题呼应（链式召回 vs Transformer 工作记忆），可对照阅读：`收藏库/教程/为什么AI会“选择性失忆”：搞懂Transformer架构.md`

## 提炼

- one_liner：从实践视角拆解 DSH：Super-agent 的上下文、架构与生态
- 各节要点：
  - 前言：DSH 即 DeepSeek Harness，源于《Super-agent 设计探索》；文章价值在把零散想法串成清晰描述，而非提出新概念。
  - 一个被当成产品问题的问题：官方观察"一个人通常只能同时跟进 3 到 5 个活跃 Session"；单任务上限取决于模型能力，多任务上限取决于人的注意力；Session 应当是系统的实现细节，而不应是用户的认知负担；产品目标最大化产出、最小化注意力，技术目标是 Human out of the loop。
  - 判断的前提：上下文边界与组织：sessionless 的本质是上下文管理，Session 只是临时执行过程；Router 是控制层/意图识别器，只决策不执行；Router 可行前提是上下文有边界（群/频道天然自带工作区边界）；链式召回把窗口当 Cache，先送摘要与 ID、按需逐跳取细节。
  - 事件如何成为一件工作：渗透需维护目标、已知情报、下一步拆解，即工作维度状态机；Source/Thread/Eval/Policy/Ticket/Mandate/Subscription/Assignment/Executor/Channel 各司其职；需同时保存事件关系与工作约定，才能持续关注且不越界。
  - 回到系统架构本身：五个模块支撑连续、高效、主动、持久四特征；双层架构本质是控制面与数据面分离，Worker 即 Agent as a Service；事件驱动用 Trigger 投喂、订阅直通，避免 Router 成为瓶颈；全双工用异步 start()/stop() 解耦前台与后台，IM 单次回复≤100 词。
  - 让Super-agent更加Super的一点设想：Agent 缺身份、感知、界面三样东西；生态不是功能有多少，是有多少系统愿意认这个身份；Trigger 本质是感知力，连接器既要能被调用还要是传感器；真正的 Super 在于能连上多少东西，解法一半在架构一半在生态。
- 带走三句话：
  - Super-agent 靠系统架构接走记忆、调度、审批、恢复，把 Session 变成实现细节。
  - Router 可行前提是上下文有边界；链式召回把窗口当 Cache，按需逐跳取细节。
  - 真正的 Super 不在模型多聪明，而在能拿到多少身份、连上多少系统。
