---
title: "一文讲透MCP的原理及实践"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247548305&idx=1&sn=05dd9222ee2077106ed35c018452ff67"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent框架, MCP]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/一文讲透MCP的原理及实践.html"
---

# 一文讲透MCP的原理及实践

> 一句话定位：MCP 是 AI 连接外部工具与数据的统一协议标准，像 USB-C 万能接口。

## 摘要

MCP (Model Context Protocol) 由 Anthropic 主导，是开放的通用协议标准，像 USB-C 统一连接数据源与工具，替代碎片化 Agent 集成，采用客户端-服务器架构。相比 function call 的平台依赖，MCP 更统一、开放、安全。模型靠 prompt 中的工具描述选工具，结果回传后生成回复。实践用 uv 和 FastMCP 写统计桌面 txt 的 Server，调试后接入 Claude Desktop。工具文档很关键，MCP 早期但生态潜力大。

## 要点

- MCP 是 Anthropic 主导的开放通用协议标准，像 USB-C 一样统一 AI 与数据源、工具的连接。
- 采用客户端-服务器架构，由 MCP 主机、客户端、服务器和本地/远程资源构成。
- 相比 function call 的平台依赖与 API 差异，MCP 提供更统一、开放、安全、灵活的工具调用机制。
- 模型靠 prompt 中的工具描述选择工具：把工具名、docstring、参数描述格式化后传给 LLM。
- 工具执行结果会与 system prompt、用户消息一起回传模型生成最终回复，无效 tool call 会被 skip。
- 工具文档至关重要：工具名称、docstring、参数说明要精心编写，选择机制基于 prompt 实现。
- 实践路径：uv 搭 Python 环境 + FastMCP 的 @mcp.tool() + mcp dev 调试 + 接入 Claude Desktop。
- MCP 仍处发展初期，统一标准下的生态会正向促进整个领域发展。

## 提炼

> 一句话定位：MCP 是 AI 连接外部工具与数据的统一协议标准，像 USB-C 万能接口。

### 分节提炼

- **01 LLM 构建 MCP 实践**：先引入 domain knowledge：复制官方文档与 SDK 内容作为 context 输入；该最佳实践基于 Claude，也可基于其它 LLM
- **02 描述需求**：需求要说明开放资源、提供工具、引导建议和对接的外部系统；example prompt 示例：连接 PostgreSQL、开放表结构、只读 SQL 工具
- **03 手动实践**：uv 搭建 Python 环境，FastMCP 用 @mcp.tool() 定义工具；mcp dev 启动 Inspector，在 localhost:5173 测试；配置 claude_desktop_config.json 接入 Claude Desktop；uv 用 which uv 取绝对路径写入 command
- **04 调试 MCP Server**：Debug 复杂，官方推荐 Debugging 与 Inspector 两个教程；直接参考官方文档链接即可，不必自己摸索

### 带走三句话

1. 选工具靠 prompt：把工具名、docstring、参数描述格式化成文本给 LLM，模型据此决定调用并输出 JSON。
2. 工具描述就是契约：精心写函数名、docstring 和参数说明，能提供工具描述的模型都能与 MCP 兼容。
3. 上手：uv 搭环境 + FastMCP 的 @mcp.tool() + mcp dev 调试 + 接入 Claude。

### 知识卡

跨全文挖掘 10 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
