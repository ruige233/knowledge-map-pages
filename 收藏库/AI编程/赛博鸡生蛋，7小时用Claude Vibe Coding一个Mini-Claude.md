---
title: "赛博鸡生蛋，7小时用Claude Vibe Coding一个Mini-Claude"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247559624&idx=1&sn=fc5df385231f0b0418ecfd127158faf2"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI编程"
tags: [AI编程]
lang: zh
status: ok
阅读页: "阅读页/AI编程/赛博鸡生蛋，7小时用Claude Vibe Coding一个Mini-Claude.html"
---

# 赛博鸡生蛋，7小时用Claude Vibe Coding一个Mini-Claude

> 一句话定位：7小时Vibe Coding手搓Mini-Claude，还原Agent交互链路

## 摘要

作者以Vibe Coding方式调用KIMI官方API，用不到7小时实现一个模拟Claude CLI的精简版Mini-Claude。开发依次打通LLM API、实现Write/Read/Exec工具、组装session对话与tool call循环、做CLI与Markdown渲染，再用MVC重构并搭建dashboard解析session日志。文中点明消息组装要命中KV Cache、tool loop本质是读改写循环，并把MCP/Skills、并发调用、上下文压缩列为后续方向。

## 要点

- 用Vibe Coding方式直调KIMI官方API，7小时内实现一个模拟Claude CLI的精简版Mini-Claude。
- 开发顺序：先打通LLM API，再实现工具调用，后组装对话与tool loop，最后做CLI界面。
- 消息组装保持前缀不变，把工具调用与上轮回复追加到message末尾以命中KV Cache。
- tool loop本质是读改写循环，直到LLM不再返回工具调用才把文本结果交给用户。
- 用cc-viewer抓包查看Claude请求明文，header需模拟Claude请求以通过KIMI后端校验。
- 重构先让LLM出方案再执行，并用MVC分层梳理结构，可减少LLM乱搞事。
- Agent核心机制可归纳为WHILE循环：取工具调用、执行、把结果返回LLM。
- MCP/Skills、并发工具调用、上下文压缩是后续方向，建议半自动开发加手动约束。

## 提炼

> 一句话定位：7小时Vibe Coding手搓Mini-Claude，还原Agent交互链路

### 分节提炼

- **01 背景**：Vibe Coding火热，但Agent Loop、ReAct等机制还需动手实践；作者决定直调原始LLM API，体验Coding Agent完整行为链路；受一篇文章启发，用Vibe Coding模拟Claude实现Mini-Claude
- **02 工程简介**：Mini-Claude模拟Claude CLI，实现CLI/Chat交互、Tool循环与上下文组装；目标是构建轻量版Copilot，三个夜晚碎片时间累计不足7小时；代码仓库q43607238/mini-claude，docs含保姆级注释；配置ApiKey与模型后启动app.js；dashboard用tnpm install安装依赖
- **03 基础演示**：基础任务演示：让Agent修改指定代码；与进阶、高阶任务共同展示Mini-Claude的实际使用效果
- **04 进阶演示**：进阶任务演示：让Agent批量修改代码；高阶任务为使用dashboard查看session日志
- **05 开发前置准备**：安装cc-viewer抓包，直接查看Claude请求明文数据；准备Coding模型，作者采用KIMI官方KIMI 2.5作开发主力
- **06 开发流程**：按LLM API、工具、对话组装与tool loop、CLI、重构、dashboard的顺序推进；消息组装保持前缀不变，把工具调用和上轮回复追加到末尾以命中KV Cache；tool loop即读改写循环，直到LLM不再返回工具调用才回复用户；重构先让LLM出方案再执行，可减少LLM乱搞事
- **07 总结**：Mini-Claude实现CLI交互、LLM交互与工具调用的最小闭环，支持简单Tool Loop；TODO：MCP/Skills、并发调用、上下文压缩是后续方向；作者认为半自动开发加手动约束Agent职责加把控方向最高效；Agent核心机制可归纳为WHILE循环：取工具调用、执行、返回LLM

### 带走三句话

1. 用Vibe Coding直调LLM API，按API、工具、循环、界面、重构的顺序迭代，就能搭出Mini-Claude。
2. Coding Agent的骨架是读改写tool loop：保持前缀命中KV Cache，直到不再返回工具调用。
3. 先让LLM出方案再改代码，并用MVC分层和模块化约束AI范围，是控制Agent不跑偏的关键。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
