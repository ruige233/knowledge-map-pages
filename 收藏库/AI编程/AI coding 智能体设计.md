---
title: "AI coding 智能体设计"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA%3D%3D&mid=2247556801&idx=1&sn=05c6adb69298f1bedfc53d047dd1ff67"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI编程"
tags: [AI编程, 智能体]
lang: zh
status: ok
阅读页: "阅读页/AI编程/AI coding 智能体设计.html"
---

# AI coding 智能体设计

> 一句话定位：从Gemini-CLI源码看AI coding智能体的提示词、工具与扩展设计

## 摘要

本文从 Gemini-CLI 源码出发，拆解 AI coding 智能体设计。提示词预处理：斜线命令、@路径读取文件、内置命令、MCP 与扩展提示词命令、TOML 自定义命令。工具注册与调用：核心工具、子智能体、用户自定义工具、MCP 工具统一注册，工具列表整体作为上下文传给大模型，MCP 工具过多会引发 token 爆炸。架构设计：意图识别与路由靠提示工程而非硬编码，主流程与子智能体都跑 ReAct 循环，并有记忆压缩。随后摘录主系统、压缩与代码库调查子智能体三类提示词。最后对比 Gemini-CLI 与 Claude Code 扩展能力，解读 OpenSpec 规约驱动的 proposal/apply/archive 三阶段，提出规约与提示词正成为项目核心资产。

## 要点

- 提示词预处理：以 / 开头视为命令，@路径 提前读取文件作为上下文，减少会话轮次。
- 命令分四类：内置命令、MCP 提示词命令、扩展包命令、本地 TOML 自定义命令。
- 核心工具、子智能体、用户自定义工具、MCP 工具统一注册，工具列表整体作为上下文提供给大模型。
- 子智能体 CodebaseInvestigatorAgent 被封装为只读工具，上下文隔离，体现高内聚低耦合。
- MCP 工具广播过多会导致上下文 token 爆炸，Claude Code 的 Skills 与 Code Execution with MCP 即为此而生。
- 意图识别与智能路由靠提示工程而非硬编码，主流程与子智能体均采用 ReAct 循环。
- 记忆压缩：超过阈值后保留最近 30% 历史，其余由模型总结，压缩后 token 增加则判失败。
- OpenSpec 规约驱动开发：proposal、apply、archive 三阶段，规范是真相、变更是提案。

## 提炼

> 一句话定位：从Gemini-CLI源码看AI coding智能体的提示词、工具与扩展设计

### 分节提炼

- **01 提示词预处理**：以 / 开头视为命令，@路径 先读文件再发送，减少模型会话轮次；内置命令在 packages/cli/src/ui/commands/ 定义，如 /clear 清空上下文、/init 生成 GEMINI.md；MCP 提示词作为扩展命令，扩展包 commands/ 目录提供带 [extension] 标识的命令；在 ~/.gemini/commands 或项目 .gemini/commands 下建 *.toml 可自定义命令
- **02 工具注册与调用**：核心工具经 createToolRegistry 注册，可用配置 coreTools 限制工具访问；子智能体 CodebaseInvestigatorAgent 封装为只读工具，上下文隔离；MCP 支持 stdio/SSE/streamable HTTP 三种协议，includeTools/excludeTools 过滤；工具列表作为上下文传入大模型，MCP 工具过多会 token 爆炸
- **03 架构设计**：意图识别与智能路由靠提示工程实现，没有硬编码路由；主流程 ReAct：Reasoning、Acting、Observing、Updating 循环，无工具调用时退出；子智能体 ReAct 有 max_turns(15)、max_time_minutes(5) 等终止条件；记忆压缩保留最近 30% 历史，压缩后 token 增加则判失败
- **04 预置提示词**：主系统提示词由 preamble、coreMandates、primaryWorkflows* 等部分组成；可用 GEMINI_PROMPT_* 环境变量关闭相关提示词；记忆压缩提示词把历史总结为结构化 XML 快照；/init 用预置提示词分析工程生成 GEMINI.md
- **05 能力扩展**：Gemini-CLI 支持命令、MCP、工具、子智能体、插件、记忆管理扩展；Claude Code 支持命令、MCP、Hooks、Skills、子智能体、插件、记忆与 SDK；Claude Code 的 Skills 懒加载 SKILL.md，减少上下文开销；Claude Code 用 Code Execution with MCP 解决工具广播与中间数据传递问题
- **06 规约驱动开发**：OpenSpec 通过 openspec/AGENTS.md 与 project.md 接入 AI coding 工具；三阶段：proposal 创建提案、apply 实施、archive 归档；规范增量用 ADDED/MODIFIED/REMOVED/RENAMED Requirements，每个需求至少一个 Scenario；规范是真相、变更是提案，MODIFIED 需粘贴完整需求，否则归档丢失细节

### 带走三句话

1. 写提示词时用 @路径 提前注入文件、用自定义命令固化高频动作，能显著减少会话轮次。
2. 慎挂 MCP：工具整体塞进上下文，越多越费 token，优先用 Skills 懒加载或编码调用 MCP。
3. 把规约沉淀为 OpenSpec/GEMINI.md 类仓库文件，用 proposal→apply→archive 管住 AI 编码全流程。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
