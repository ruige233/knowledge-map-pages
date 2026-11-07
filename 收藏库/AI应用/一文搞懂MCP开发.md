---
title: "一文搞懂MCP开发"
url: "https://mp.weixin.qq.com/s/8qB8xcp8d3yYXnGZNlutEA"
source: "腾讯云开发者"
published: "2026-11-03"
archived: "2026-11-03"
category: "AI应用"
tags: [Agent 研发, MCP]
lang: zh
status: ok
阅读页: "阅读页/AI应用/一文搞懂MCP开发.html"
---

# 一文搞懂MCP开发

> 一句话定位：MCP 用标准协议连接 AI 应用与外部系统，把集成成本从 M×N 降到 M+N。

## 摘要

本文系统讲解 Model Context Protocol（MCP）开发：它由 Anthropic 于 2024 年底开源，在 AI 应用与外部系统之间插入一层标准协议，把「3 个 AI 应用 × 3 个数据源 = 9 套集成」降为 M+N。文章按「为什么需要 MCP → 核心概念 → 本地 vs 远程」展开：先厘清 MCP 与 Function Calling、Agent/Skill/CLI 的分工与适用边界；再讲 Host/Client/Server 三角色、JSON-RPC 数据层 + 传输层两层架构、能力协商，以及 Tools/Resources/Prompts 三种原语与 Sampling/Elicitation 反向能力；最后对比 stdio 与 Streamable HTTP 两种传输，剖析 SSE 长连接被弃用的原因，并以 Vercel MCP 为例说明远程模式落地。安全模型强调双向不信任：Host 不信任 Server，Server 不信任 Client，Tool Annotations 只是建议而非约束。基于 MCP 规范 2026-07-28 与 TypeScript SDK v2。

## 要点

- MCP 是面向 AI 应用的协议而非终端用户 API：Server 暴露 tools/list、tools/call 等标准接口，Host 实现一次 Client 即可复用。
- Function Calling 定义「模型如何表达调用意图」，MCP 定义「调用意图如何到达外部系统」，二者串联而非替代。
- Tools（模型驱动）、Resources（应用注入）、Prompts（用户选择）三种原语按控制权归属区分；Sampling/Elicitation 反向能力控制权始终在 Host。
- 所有客户端请求必须携带 _meta（协议版本 + clientInfo），缺失报 -32602；2026-07-28 起协议无状态，跨请求状态由 Client 显式传递。
- stdio 消息逐行 JSON-RPC，console.log 会破坏协议（日志用 stderr）；Streamable HTTP 无状态默认、会话可选，替代了仅存活 8 个月的 SSE 长连接方案。
- 安全上 Host 不信任 Server、Server 不信任 Client；Tool Annotations 的 Hint 后缀是建议而非约束。

## 关联

- 与《AI coding 智能体设计》（收藏库/AI编程/AI coding 智能体设计.md）相关：均涉及 AI 应用与工具/外部系统的编排边界。
- 与《打造高可靠 AI 助手：Skill 编排、Workflow 设计与 Spec Coding 的深度实践》（收藏库/AI编程/打造高可靠 AI 助手：Skill 编排、Workflow 设计与 Spec Coding 的深度实践.md）相关：本文 1.3 节明确了 Skill 与 MCP 的分工。

## 提炼

- 一句话定位：MCP 用标准协议连接 AI 应用与外部系统，把集成成本从 M×N 降到 M+N。
- 带走三句话：
  1. 先想清是否真的需要 MCP：有模型参与、能力需多应用共享、底层 API 稳定，三者齐备再引入。
  2. 把 Tools/Resources/Prompts 的控制权想清楚，安全决策交给 Host 策略引擎而非 Server 注解。
  3. 本机能力走 stdio，共享与第三方服务走 Streamable HTTP；日志永远用 stderr。
- 各节要点：
  - 什么是 MCP：M×N 降为 M+N；与 Function Calling 串联分工；Agent/Skill/MCP/CLI 选型原则；适用与不适用场景。
  - 核心概念：三角色、两层架构、能力协商、三种原语、Sampling/Elicitation、Tool Annotations、信任边界。
  - 本地 vs 远程：stdio 与 Streamable HTTP；弃用 SSE 长连接的原因；Vercel MCP 案例。
  - 总结：能力暴露、安全模型、部署模式三个层面。
