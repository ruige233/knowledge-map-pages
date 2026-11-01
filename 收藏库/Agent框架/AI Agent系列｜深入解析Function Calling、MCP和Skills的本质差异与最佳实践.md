---
title: "AI Agent系列｜深入解析Function Calling、MCP和Skills的本质差异与最佳实践"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247558457&idx=1&sn=088df2d9371481bb54961b09688c6796"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent框架, Agent, MCP, Skill]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/AI Agent系列｜深入解析Function Calling、MCP和Skills的本质差异与最佳实践.html"
---

# AI Agent系列｜深入解析Function Calling、MCP和Skills的本质差异与最佳实践

> 一句话定位：厘清 Function Calling、MCP、Skills 三者本质差异与最佳实践

## 摘要

本文解析 Function Calling、MCP 与 Skills 的本质差异。Function Calling 是工具调用基础：LLM 把非结构化需求转为结构化 JSON，系统据此执行函数并回传结果。MCP 与 Skills 都基于它：MCP 用 JSON-RPC 与 Server 标准化接入既有系统；Skills 用文字定义指令、脚本和资源。二者是竞争而非互补。Skills 是 sub-agent 包装，适应性强但无法 100% 准确。Lynxe 的解法是把 Agent 能力函数化。

## 要点

- Function Calling 是 AI Agent 调用工具的基础能力，核心是让 LLM 输出结构化 JSON（tool_calls、function.name、function.arguments）。
- 工具调用本质：把非结构化用户需求转换为结构化函数调用，与外部系统交互后再把结果回传模型决策。
- MCP 基于 Function Calling，通过 JSON-RPC 协议与 Server 把既有系统标准化接入 LLM，降低集成成本。
- Skills 同样基于 Function Calling，用文字定义指令、脚本和资源，让模型自动发现并加载 SKILL.md 执行流程。
- MCP 与 Skills 是竞争而非互补：协议转换加代码串接 vs 命令行调用加文字流程定义，解决同一多系统整合问题。
- Skills 本质是 sub-agent 的包装，优势是流程环境适应性强，代价是结果无法 100% 准确。
- Skills 仍有两个问题：需求描述不够结构化、Agent 只能以聊天方式接驳既有系统。
- Lynxe 的答案是“一切都是函数、函数才是第一公民”，把 Agent 能力函数化以便集成进业务系统。

## 提炼

> 一句话定位：厘清 Function Calling、MCP、Skills 三者本质差异与最佳实践

### 分节提炼

- **01 接收请求与工具描述**：系统把可用工具列表和描述连同用户请求一起交给 LLM；工具描述是 LLM 判断是否需要调用工具的依据
- **02 LLM 决策是否调工具**：LLM 需要调用工具时返回结构化 JSON，而非自然语言；关键字段：tool_calls、function.name、function.arguments；结构化输出让系统稳定解析意图，免去复杂文本解析
- **03 解析并执行工具**：系统按 function.name 找到函数、解析 function.arguments 后执行；JS 示例：从 tool_calls 取参数后调用 tools[functionName](args)；整个过程自动化，使工具调用可预测、可靠
- **04 结果回传 LLM**：执行结果返回给 LLM，由其决定继续调用或生成最终回答；工具调用本质：在非结构化需求与结构化系统之间架桥；Function Calling 是 MCP 与 Skills 存在的前置条件
- **05 Skills 描述不够结构化**：仅靠 description 字段描述 sub-agent 要求，信息易不准确；任务复杂度增加时纯文本描述的不确定性会被放大
- **06 Agent 难接驳既有系统**：仅靠聊天方式接驳，Agent 最终只能是个对话框；真实系统输入方式远不止 textarea，难以集成表单、实时数据与 UI；Lynxe 思路：一切都是函数、函数是第一公民，把 Agent 能力函数化暴露

### 带走三句话

1. 工具调用的本质是 LLM 把非结构化需求转为结构化函数调用，MCP 和 Skills 都只是这一基础能力上的不同应用。
2. MCP 与 Skills 是竞争而非互补：接驳协议加代码串接 vs 命令行调用加文字流程定义。
3. 用文字定义流程能换来环境适应性，但代价是无法 100% 准确，落地还需把 Agent 能力函数化。

### 知识卡

跨全文挖掘 10 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
