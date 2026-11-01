---
title: "自媒体创作场景实践｜通义千问3 + MCP=一切皆有可能"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247549555&idx=1&sn=ea36ae41c6e3247c4518a8d7a988b49e"
source: "微信公众平台 · 等媒体社交平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent框架, MCP]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/自媒体创作场景实践｜通义千问3 + MCP=一切皆有可能.html"
---

# 自媒体创作场景实践｜通义千问3 + MCP=一切皆有可能

> 一句话定位：Qwen3经MCP串起文件、ComfyUI与小红书，自动生成发布图文。

## 摘要

本文用通义千问Qwen3-235B-A22B经CLine接入OpenAI兼容接口，挂载Filesystem、自建ComfyUI与小红书三个MCP Server，实现从本地读prompt、ComfyUI文生图下载、再生成文案发布小红书的自动化流程，并给出两个Server的FastMCP实现要点与配置。作者认为MCP本质是打通数据IO与物理世界交互，可拓展到售卖API、订单分析、脑机与机器人场景；也指出模型不够智能、开发有坑、安全风险等不足，建议用FastMCP、命名明确、加注解、给默认值。

## 要点

- 用Qwen3-235B-A22B作大模型，经CLine接入DashScope的OpenAI兼容接口并串联多个MCP Server。
- 实践场景为：从本地文件读prompt→ComfyUI文生图→下载到本地images→生成文案发布小红书。
- ComfyUI MCP Server用固定workflow，经HTTP提交任务、WebSocket监听完成、/view接口取图下载。
- 小红书Server用Playwright执行签名、XhsClient发布笔记，演示版经social-auto-upload的/create接口发布。
- CLine通过cline_mcp_settings.json注册filesystem(stdio)、comfyui(SSE)、xiaohongshu(SSE)三个Server。
- 作者认为MCP本质是打通数据IO，让大模型连接本地、网络、数据库与媒体平台等物理世界交互。
- 不足：模型多步易错、FastMCP2.0开发坑多、安全风险（鉴权、本地文件、非原子操作、Secret泄露）。
- 开发建议：使用FastMCP框架，tool命名明确、加prompt注解注释、参数给默认值。

## 提炼

> 一句话定位：Qwen3经MCP串起文件、ComfyUI与小红书，自动生成发布图文。

### 分节提炼

- **01 安装CLine**：在VSCode中安装CLine插件，作为接入通义千问的客户端。；安装完成后即可继续把CLine接入Qwen3接口。
- **02 接入Qwen3**：接入模式选择“OpenAI Compatible”。；Base URL填DashScope的compatible-mode/v1地址。；Model ID填qwen3-235b-a22b。
- **03 售卖Server API**：供应商可把复杂商业非开源产品的内部API封装成MCP Server。；客户通过MCP Server调用，形成可售卖的服务。
- **04 订单自动化**：电商订单API接入MCP后，大模型可自动分析订单。；还可制作报表、管理商家商品并灵活调整售价。
- **05 脑机与机械臂**：MCP可为残障人群定制化场景。；通过MCP+大模型完成游戏、工作等任务。
- **06 具身智能机器人**：移动机器人可做大模型驱动的覆盖路径规划。；手术机器人理解复杂手术环境和任务要求。；智能家居机器人理解并执行家庭任务。
- **07 模型不够智能**：最好的大模型仍会失误，一步错可能步步错。；精细化任务难处理，如blender指令常因版本知识库过旧而出错。
- **08 Server开发有难度**：基于FastMCP2.0开发代码量不大。；网络、依赖冲突等坑多，需自行解决且网上资料很少。
- **09 安全风险**：身份验证机制还不完善。；Filesystem server可能误修改本地文件。；大模型翻译后的指令可能错误甚至带来风险。；非原子操作与本地Secret数据存在资金损失和泄露风险。
- **10 命名明确**：tool定义的函数和参数命名尽可能明确。；命名明确便于mcp list tool时理解各tool功能。
- **11 加注解注释**：函数内部加tool功能的prompt注解和注释。；选择tool时对每个tool的功能有更明确的理解。
- **12 参数带默认值**：参数尽量附带默认值。；防止调用的时候漏掉参数。
- **13 解决安全问题**：安全类问题亟待解决。；否则商业场景会存在很大不可靠性。

### 带走三句话

1. Qwen3+CLine接DashScope，挂Filesystem、ComfyUI、小红书即打通图文发布链路。
2. ComfyUI走HTTP提交+WebSocket等完成信号，小红书靠Playwright签名，FastMCP开发更简洁。
3. MCP能接一切API，但模型多步易错、非原子操作与Secret泄露是商业落地短板。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
