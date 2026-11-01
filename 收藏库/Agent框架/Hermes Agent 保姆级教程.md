---
title: "Hermes Agent 保姆级教程"
url: "https://mp.weixin.qq.com/s?__biz=MzkwNjMwMTgzMQ==&mid=2247538222&idx=1&sn=a03c843ce985ef460055ca1e308f28c5"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent框架, Agent]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/Hermes Agent 保姆级教程.html"
---

# Hermes Agent 保姆级教程

> 一句话定位：Hermes Agent 从安装、进阶配置到变现的保姆级教程

## 摘要

Hermes Agent 是比 OpenClaw 更稳定、省钱且会自我进化的 Agent。教程分三阶段：新手入门讲一行命令安装、doctor/chat 验证与 DeepSeek/Kimi/OpenRouter 免费模型；进阶配置讲 8 模块分模块配模型省 token、SOUL.md 人格、5 个记忆文件、接入飞书与 Telegram；高级玩法讲 Skill 沉淀、Cron 定时任务与 openclaw 迁移。最后补充社区资源和代安装、行业定制、卖 Skill 包、自动化业务四种变现路径。

## 要点

- Hermes 定位为更高级的 OpenClaw，更稳定、省钱、能自我进化，GitHub 近 9 万 Star。
- 安装仅需一行 curl 命令，Windows 先装 WSL2，装完用 hermes version、hermes doctor 验证。
- 新手模型配置选 Quick Setup，推荐 DeepSeek、Kimi 或 OpenRouter 免费模型，用 hermes chat 测试。
- 任务拆成 8 个模块可单独配模型，核心模块用 Claude Opus/GPT-4o，杂活用免费模型，作者省了六七成 token 费用。
- SOUL.md 是人格文件，可手写或让 Hermes 根据对话自动生成；记忆系统由 5 个文件组成，只需维护 SOUL.md。
- 通过 hermes gateway setup 接入飞书、Telegram 等 15+ 平台，实现 24 小时在线助理。
- Skill 系统是 Hermes 与 OpenClaw 最本质的区别，完成任务后自动沉淀可复用技能。
- 内置 Cron 定时任务，排错用 hermes doctor，迁移用 hermes migrate openclaw。

## 提炼

> 一句话定位：Hermes Agent 从安装、进阶配置到变现的保姆级教程

### 分节提炼

- **01 新手入门**：macOS/Linux/WSL2 用一行 curl 命令安装，Windows 需先装 WSL2。；安装会自动处理 Python、Node.js、Git 依赖并创建 ~/.hermes/ 配置目录。；新手选 Quick Setup，模型可用 DeepSeek、Kimi 或 OpenRouter 免费模型。；常用命令：hermes、hermes model/tools/gateway/setup/doctor、hermes migrate openclaw。
- **02 进阶配置**：任务拆成 8 个模块可单独配模型，核心模块用强模型、杂活用免费模型。；作者调完 token 费用省了六七成，效果基本没打折扣。；SOUL.md 定义人格，可手写或让 Hermes 根据对话自动生成。；接入飞书/Telegram 走 hermes gateway setup 或改 config.yaml。
- **03 高级玩法**：Skill 系统是 Hermes 与 OpenClaw 最本质的区别，完成任务自动沉淀经验。；触发建技能：完成多步骤任务后、主动说创建技能、跨会话重复工作模式。；定时任务编辑 ~/.hermes/cron/tasks.yaml，用 Cron 调度并推送到平台。；hermes doctor 定位问题，hermes migrate openclaw 一键迁移。
- **04 社区资源**：hermes-web-ui 不用改配置即可用，支持 CLI/Telegram/微信切换。；技能商店现成技能直接安装，地址为 hermes-agent.nousresearch.com/docs/skills。；中英文文档齐备，入门看中文文档即可。
- **05 变现**：四种变现：代安装、垂直行业定制、卖 Skill 包、让 Agent 跑自动化业务。；行业定制客单价高于代安装；Skill 是 Markdown 文件，交付成本低。；核心竞争力是对 Hermes 的理解深度，先玩透先吃红利。

### 带走三句话

1. 先跑通新手阶段：一行命令安装、doctor/chat 验证、配免费模型，再谈进阶。
2. 省钱关键在分模块配模型，核心模块用强模型、杂活挂 OpenRouter 免费模型。
3. 想接单变现，先吃透 SOUL.md、Skill 系统和 gateway，理解深度才是护城河。

### 知识卡

跨全文挖掘 9 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
