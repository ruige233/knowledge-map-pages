---
title: "DeepSeek V4+Claude Code一手实战！夯爆了还是拉完了？"
url: "https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247554204&idx=1&sn=87ada30fdb5c2bcdd9ad1c9f5f979eef"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI编程"
tags: [AI编程, Claude Code, DeepSeek]
lang: zh
status: ok
阅读页: "阅读页/AI编程/DeepSeek V4+Claude Code一手实战！夯爆了还是拉完了？.html"
---

# DeepSeek V4+Claude Code一手实战！夯爆了还是拉完了？

> 一句话定位：DeepSeek V4 Pro/Flash 接 Claude Code 三场实战与定价实测

## 摘要

DeepSeek V4 Pro/Flash 经 Anthropic 兼容接口接入 Claude Code 实测：配 Tavily 一遍升级模型预设；诊断 SQL 未执行并集成 Flyway，踩中 Spring Boot 4.x 拆包坑；Flash 驱动面试平台效果不错。Pro 1.6T/49B、Flash 284B/13B，SWE-bench 80.6% 接近 Opus 4.6。结论：Agent Coding 上台阶，Pro 偏贵、Flash 暂非主力，复杂任务仍不及 Claude/GPT。

## 要点

- DeepSeek V4 提供 Anthropic 兼容接口，Claude Code 可直接对接，无需第三方适配层。
- 接入两种方式：改 ~/.claude/settings.json 的 env 字段，或用 CC Switch 可视化切换。
- 实战一：配 Tavily 搜索后，V4 Pro 一遍完成多 Provider 模型预设升级（改三个文件）。
- 实战二：V4 Pro 定位 V2 SQL 未挂载容器，引入 Flyway 并两轮调试修复成功。
- Spring Boot 4.x 自动配置拆分，Flyway 需用官方 Starter，只引 flyway-core 会静默失败。
- 实战三：V4 Flash 非思考模式生成模拟面试题与答案，性价比高。
- 参数：Pro 1.6T/49B、Flash 284B/13B，均 1M 上下文、MIT；SWE-bench 80.6% 接近 Opus 4.6。
- 定价：Flash 输出 $0.28/M、Pro $3.48/M，约为 Sonnet 4.7 的 1/50 与 1/4。

## 提炼

> 一句话定位：DeepSeek V4 Pro/Flash 接 Claude Code 三场实战与定价实测

### 分节提炼

- **01 接入 DeepSeek V4**：Claude 官方模型太贵且易封号，DeepSeek 的 Anthropic 兼容接口可直连。；配置文件法：~/.claude/settings.json 写 BASE_URL、MODEL、AUTH_TOKEN 等 env。；CC Switch 可多 Provider 一键切换，填 Base URL 与 API Key 即可。；启动后 /status 确认 model 为 DeepSeek-V4-Pro 即接入成功。
- **02 实战一：升级模型预设**：不配 Tavily 时靠模型记忆猜最新版本易出错。；V4 Pro 一次完成搜索、更新三家模型并改三个文件。；更新后 DashScope/DeepSeek/GLM/Kimi 预设均切到最新模型。
- **03 实战二：诊断迁移并集成 Flyway**：V2__knowledge_skill.sql 未挂载容器且无迁移工具，init.sql 由 Compose 写死执行。；集成 Flyway 按命名约定管理迁移顺序，两轮调试后修复。；Spring Boot 4.x 将 FlywayAutoConfiguration 拆到 spring-boot-flyway 模块。
- **04 实战三：面试平台接 Flash**：面试平台已支持多模型切换，默认模型切到 deepseek-v4-flash。；Flash 非思考模式生成模拟面试题与答案，质量不错。
- **05 V4 参数与定价**：Pro 1.6T/49B、Flash 284B/13B，均 1M 上下文、MIT 开源。；SWE-bench Verified 80.6% 接近 Opus 4.6 的 80.8%，价格便宜两个数量级。；Flash 输出 $0.28/M、Pro $3.48/M，远低于 Sonnet 4.7。
- **06 总结**：V4 在 Agent Coding 与代码理解上明显上台阶。；Pro 无 Coding Plan 价格偏高，Flash 暂非日常开发主力。；复杂编码、精准问答与前沿推理仍不及 Claude Opus 4.6 / GPT 5.5。

### 带走三句话

1. Claude Code 接 DeepSeek 走 Anthropic 兼容接口：改 settings.json 或用 CC Switch。
2. Spring Boot 4.x 集成 Flyway 用官方 Starter，只引 flyway-core 会静默失败。
3. Flash 便宜能打但非开发主力，复杂任务换 Claude/GPT；Pro 无 Coding Plan 偏贵。

### 知识卡

跨全文挖掘 10 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
