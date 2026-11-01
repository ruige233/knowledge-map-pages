---
title: "学习笔记：从 Agent 到 Skills — AI 智能体架构的范式转变"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247559249&idx=1&sn=7cda1453a5f7f51c39f43b76027696a1"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent框架, Agent, Skill, 智能体]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/学习笔记：从 Agent 到 Skills — AI 智能体架构的范式转变.html"
---

# 学习笔记：从 Agent 到 Skills — AI 智能体架构的范式转变

> 一句话定位：相册自动化项目拆解 Agent、Skills、MCP、OpenClaw 分层协作

## 摘要

本文记录 Anthropic 14 个月内推出 MCP 与 Agent Skills 两个开放标准，把智能体从单体 Agent 推向“薄 Agent + 可组合 Skills”的分层架构。作者用“自动化美化相册”项目拆解 Agent、Skills、MCP、OpenClaw、A2A 五个概念，对照 Claude Code 硬编码与 OpenClaw Agent 调度两种实现。结论：知识用 Skills 表达、动态连接用 MCP、运行交给 OpenClaw，换环境时 Skills 与 MCP 不变。

## 要点

- Anthropic 14 个月内先后开源 MCP 与 Agent Skills，Skills 成为开放标准后 48 小时内被 Microsoft、OpenAI 采纳
- Skills 是模块化知识包，通过 SKILL.md 元数据→指令→资源三层渐进式披露，按需加载省上下文窗口
- MCP 是标准化工具连接协议（JSON-RPC 2.0），负责动态数据与外部服务；Skills 教 Agent 怎么用工具，两者互补
- OpenClaw 是开源 Agent 运行环境，提供 Gateway、Agent 推理、Skill 发现、MCP 编排与 Memory
- 真实项目用 Director-Creator-Critic 的 Prompt 编排在单个 Skill 内实现多角色协作，无需部署多个 Agent
- 实验对比：Claude Code 方案确定性高、延迟低但硬编码；OpenClaw 方案语义理解强但每次约耗 15K token、首次响应约 20 秒
- 趋势是从“造全能 Agent”转向“写可复用 Skills”，并出现 Skills 市场化、环境智能、协议分层与安全治理

## 提炼

> 一句话定位：相册自动化项目拆解 Agent、Skills、MCP、OpenClaw 分层协作

### 分节提炼

- **01 起源与行业响应**：MCP 解决外部工具/数据连接，Skills 解决领域专业知识加载；Skills 2025-12-18 发布，48 小时内被 Microsoft、OpenAI 采纳；Gartner 预测 2026 年 75% AI 项目聚焦可组合 Skills
- **02 相册自动化项目**：Ming-flash-omni-2.0 + Zenmux + OpenClaw；filesystem MCP 经 npx 安装，14 个 Tools 全部可用；wechat-mcp v2 协议层可通但实际操作不可用，改用键盘模拟；MCP Server、注册、Skill 说明书、业务脚本是分层引用关系
- **03 四个核心概念**：单体 Agent 把所有知识塞进 System Prompt，不可复用难维护；Skills 用 SKILL.md 三层渐进式披露，按需加载省上下文；Director-Creator-Critic 是同一 LLM 不同 Prompt 的角色编排；能写成脚本跑完用 Skill，需持续连接外部服务用 MCP
- **04 对比总结**：Agent/Skills/MCP/OpenClaw 分别回答谁做、怎么做、用什么、在哪做；MCP 有状态动态连接，Skills 无状态静态加载；协议栈分层：知识层 Skills、工具层 MCP、协作层 A2A、运行层 OpenClaw
- **05 实验验证**：Claude 方案是硬编码脚本，编写时智能、运行时确定；OpenClaw 方案是薄桥接 + Agent 推理，运行时 AI 决策；OpenClaw 每次请求约 15K token，首次响应约 20 秒；固定流水线选 Claude 方案，多任务自然语言入口选 OpenClaw
- **06 趋势与展望**：口号：Stop Building Agents, Start Building Skills；Skills 天然适合 Git 分发，装一个像装 npm 包；安全风险：Skill 注入、MCP 信任、Agent 越权、供应链攻击
- **07 总结**：范式本质：把知识从模型拆出，变成可单独管理的文件；换运行环境时 Skills 与 MCP 代码不用改，已被实验验证
- **08 写在最后**：代码开源在 github.com/forde450/agent-skills-report；作者为百灵多模态算法工程师

### 带走三句话

1. 把知识写成 Skills，动态连接交给 MCP，运行交给 OpenClaw，三层解耦、独立演进。
2. Claude Code 方案确定性高、延迟低；OpenClaw 方案灵活，固定流水线选前者，多任务入口选后者。
3. Skills 生态爆发的同时，注入攻击、越权与供应链风险要求审核、沙箱与签名验证。

### 知识卡

跨全文挖掘 10 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
