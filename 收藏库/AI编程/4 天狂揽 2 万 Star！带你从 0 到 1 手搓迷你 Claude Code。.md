---
title: "4 天狂揽 2 万 Star！带你从 0 到 1 手搓迷你 Claude Code。"
url: "https://mp.weixin.qq.com/s?__biz=MzUxNjg4NDEzNA==&mid=2247531659&idx=1&sn=2b7e497d7d58f668e92d04a39935f19b"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI编程"
tags: [AI编程, Claude Code]
lang: zh
status: ok
阅读页: "阅读页/AI编程/4 天狂揽 2 万 Star！带你从 0 到 1 手搓迷你 Claude Code。.html"
---

# 4 天狂揽 2 万 Star！带你从 0 到 1 手搓迷你 Claude Code。

> 一句话定位：12 步带你从 0 到 1 复刻迷你 Claude Code 的教学项目

## 摘要

本文介绍教学型开源项目 learn-claude-code：从不到 200 行代码的最小 Agent 起步，按 s01–s12 十二步复刻迷你 Claude Code，覆盖工具调用、规划、子 Agent、任务系统、异步后台任务、多 Agent 协作与任务隔离。核心理念是模型本身就是 Agent，只需给合适的工具、状态与协议并尽量少打扰。每节只引入一个核心机制，配套 Python 实现与文档讲解心智模型。文末给出克隆、配置 ANTHROPIC_API_KEY 与启动 Web 学习平台的步骤。

## 要点

- learn-claude-code 是 0→1 教学项目，用不到 200 行最小 Agent 逐步搭出迷你 Claude Code。
- 核心理念：模型本身就是 Agent，只需给合适的工具、状态与协议并尽量少打扰。
- s01–s12 每节只引入一个核心机制，配 Python 实现与对应文档，附 Motto 概括核心。
- 12 步覆盖 Agent 循环、工具扩展、规划、子 Agent、技能加载、上下文压缩等能力。
- 任务系统用 JSONL/文件持久化状态，上下文压缩后任务状态仍存活于文件系统。
- 多 Agent 通过邮箱/队列异步通信，自治 Agent 以轮询-认领-执行-写回模式自组织。
- 上手：clone 后 pip install、配置 .env 的 ANTHROPIC_API_KEY，再运行 s01 脚本或 Web 平台。
- 项目是教学内核实现而非生产级克隆，故意简化工程细节。

## 提炼

> 一句话定位：12 步带你从 0 到 1 复刻迷你 Claude Code 的教学项目

### 分节提炼

- **01 项目定位与理念**：0→1 教学项目，从不到 200 行代码的最小 Agent 搭出迷你 Claude Code。；具备工具调用、规划、子 Agent、任务系统、后台任务、多 Agent 协作与任务隔离。；理念：模型本身就是 Agent，给工具、状态与协议，尽量少打扰。；非生产级克隆，教学内核故意简化工程细节，用 Python+文档+Web 平台讲解。
- **02 12 步学习路线**：s01–s12 每节只引入一个核心机制，配 Python 实现与文档。；每节用 Motto 一句话点出该机制的心智模型。；docs 用问题→方案→ASCII 图→少量代码解释特性背后的模型。
- **03 快速上手**：clone 后 pip install -r requirements.txt 安装依赖。；cp .env.example .env，配置 ANTHROPIC_API_KEY 等变量。；从 s01_agent_loop.py 起步，可跑 s11/s12 体验完整能力。；Web：cd web 后 npm install、npm run dev 即可。

### 带走三句话

1. 想彻底掌握 Claude Code，最好亲手复刻一套类似逻辑。
2. 每节只学一个核心机制，配合 Motto 与文档消化心智模型。
3. 从 s01 最小循环跑起，再逐步体验子 Agent、多 Agent 与 worktree 隔离。

### 知识卡

跨全文挖掘 11 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
