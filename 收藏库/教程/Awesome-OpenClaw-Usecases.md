---
title: "Awesome OpenClaw Usecases"
url: "https://github.com/hesamsheikh/awesome-openclaw-usecases"
source: "GitHub / hesamsheikh"
published: "2026-02-08"
archived: "2026-10-02"
category: "教程"
tags: [OpenClaw, 用例集合, 多代理, 工作流自动化, 自托管]
lang: en
status: ok
---

# Awesome OpenClaw Usecases

> 一句话定位：社区维护的 OpenClaw 真实用例合集，针对"技能不缺、缺的是能改善生活的场景"这一采用瓶颈，把可落地的玩法分类列出来。

## 摘要

由 hesamsheikh 维护，针对 OpenClaw（前身 ClawdBot / MoltBot）推广的关键瓶颈——不缺技能，缺的是能改善生活的具体场景。用例分四类：社交媒体（Reddit / YouTube 日报、X 账号分析、聚合 109+ 来源的科技新闻摘要）、创意与构建（目标驱动的自主任务、多代理内容工厂、自主游戏开发与播客管线）、基础设施与 DevOps（用 n8n webhook 委托 API 调用使代理不接触凭证、家庭服务器自愈代理）、生产力（STATE.yaml 协调多代理、个人 CRM、电话助手、会议纪要等）。仓库警告所列第三方技能未经审核，可能存在严重漏洞。截至 2026-10-02 约 31.7k stars、2.72k forks，MIT 许可。

## 要点

- 用例按"社交媒体 / 创意与构建 / 基础设施与 DevOps / 生产力"四类归档，可直接按场景检索，而不必先读技能文档。
- 安全提示明确：清单链接的社区技能、插件与外部仓库未经维护者审核，使用前应审查源码、检查权限，并避免硬编码 API 密钥。
- 代表性能说明具体架构取舍：用 n8n webhook 委托 API 调用使代理不接触凭证；用 STATE.yaml 协调多代理并行以省去协调器开销。
- 覆盖从消费者场景（Reddit/YouTube 摘要、家庭日历）到工程场景（自愈服务器、自主游戏开发管线、播客与视频生产），可用作 OpenClaw 能力边界参考。
- 关注度：约 31.7k stars、2.72k forks；MIT 许可；GitHub topics 含 awesome-list、clawdbot、moltbot、openclaw、openclaw-plugin、openclaw-setup、openclaw-skills、usecase。

## 关联

- 与 `教程/Awesome-OpenClaw-Tutorial.md` 配套：中文教程解决部署与配置，本清单解决"用它做什么"。
- 与 `Agent框架/GenericAgent.md`、`Agent框架/OpenAI-Agents-SDK.md` 在多代理编排主题上可对照阅读。
- 与 `AI应用/OpenMAIC.md` 相关，后者也提供 OpenClaw 集成入口。

## 注意

- 源笔记中 "Phone-Based Personal Assistant" 在生产力分类下重复出现两次（描述略有差异），本条目按去重后的场景归纳，未逐条校对清单当前条目数。
- 仓库未通过 API 返回主语言（`language` 为空），故未记录主语言；星标/forks 为 2026-10-02 快照。
- 清单内的第三方技能与外部仓库均未核验，安全警告为仓库原文立场。
