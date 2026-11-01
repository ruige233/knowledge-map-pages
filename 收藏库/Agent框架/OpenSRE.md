---
title: "OpenSRE"
url: "https://github.com/Tracer-Cloud/opensre"
source: "GitHub / Tracer-Cloud"
published: "2026-01-13"
archived: "2026-10-02"
category: "Agent框架"
tags: [SRE, 事故响应, 根因分析, 可观测性, PagerDuty]
lang: en
status: ok
---

# OpenSRE

> 一句话定位：一个面向 SRE 场景的开源 AI Agent 工具包，把生产事故的上下文采集、关联分析、根因报告和通知推送串成一条自动链路。

## 摘要

OpenSRE 是面向站点可靠性工程（SRE）的开源 AI Agent 工具包，把生产事故调查与响应自动化：告警触发后自动拉取日志、指标、链路追踪等上下文做关联分析，输出带根因分析和证据链的结构化调查报告，并可推送到 Slack 或 PagerDuty。它集成了 60 多个服务，覆盖 Anthropic、OpenAI、Ollama 等 LLM 提供商，Grafana、Datadog、Honeycomb、CloudWatch、Sentry 等可观测平台，以及 K8s、AWS、GCP、Azure 等基础设施；另内置 RCA 测试套件，可用合成事故评估调查效果。仓库创建于 2026-01-13，Apache-2.0 许可，API 快照 11,328 stars、1,656 forks，主语言 Python，2026-10-02 仍有推送。

## 要点

- **事故响应闭环**：告警 → 自动采集日志/指标/Trace 上下文 → 关联分析 → 输出结构化报告（根因 + 证据链）→ 推送 Slack 或 PagerDuty。
- **集成面广**：LLM 侧含 Anthropic、OpenAI、Ollama；监控侧含 Grafana、Datadog、Honeycomb、CloudWatch、Sentry；基础设施侧含 K8s、AWS、GCP、Azure，合计 60+ 服务。
- **定位是工具包而非成品**：官方描述为「build your own AI SRE agents」，便于按自家告警源与观测栈定制调查流程。
- **可评估性**：内置 RCA 测试套件，用合成事故回放来检验根因定位质量，而不是只靠主观判断。
- **关注度**：API 快照 11,328 stars / 1,656 forks，Apache-2.0 许可，主语言 Python。

## 关联

- `Agent框架/OpenAI-Agents-SDK.md` — 通用多 Agent 编排框架，可作为自建 SRE Agent 时的编排底座对照。

## 注意

- 源笔记标注项目处于 Public Alpha 阶段且 star 数未明确；API 快照已显示 11,328 stars、1,656 forks，阶段描述可能已过时。
- 集成服务的具体数量与「自动根因分析」的效果描述来自源笔记与仓库自述，未逐项核验集成清单。
