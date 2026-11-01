---
title: "多 AI 协同 + SDD 编程实践：一个 AI 全流程交付实录"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247557693&idx=1&sn=a904d94029bc877eb7319d096308fb7b"
source: "微信公众平台 · 阿里云开发者"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI编程"
tags: [AI编程]
lang: zh
status: ok
阅读页: "阅读页/AI编程/多 AI 协同 + SDD 编程实践：一个 AI 全流程交付实录.html"
---

# 多 AI 协同 + SDD 编程实践：一个 AI 全流程交付实录

> 一句话定位：以 SDD 规范让 Claude 统筹、Codex 编码、Gemini 分析交付

## 摘要

本文记录一套基于 SDD 的多 AI 协同编码实践，从零交付跨境保险产品。核心是从 Vibe Coding 转向规范驱动：选型 OpenSpec 作轻量、工具无关、变更可管理的规范底座，经 MCP 把 Codex、Gemini 注入 Claude，形成“Claude 统筹、Codex 实现、Gemini 只读分析”铁三角。实战分六阶段：Spec-PRD、架构总结、技术方案、变更提案、变更实现与归档，实现阶段分阶段暂停人工 Review。结论：SDD 不消除 AI 不确定性，而是收敛到可验证轨道。

## 要点

- AI Coding 关键不在“写得快”而在“写得对”，需从 Vibe Coding 转向 SDD。
- SDD 以规范为契约与唯一事实源，Specify→Plan→Tasks→Implement 四阶段逐级人工确认。
- 选型 OpenSpec：轻量可嵌入、多 AI 友好、变更可管理，且不绑定单一 AI 工具。
- 通过 MCP 将 Codex、Gemini 注入 Claude，形成“Claude 统筹 + Codex 实现 + Gemini 分析”铁三角。
- 用 CLAUDE.md、AGENTS.md 强制规则与决策提醒，保证工具调用是默认行为。
- 六阶段实战：Spec-PRD→架构总结→技术方案→变更提案→变更实现→提案归档，从零交付跨境保险产品。
- 分阶段交互式开发、逐阶段暂停人工 Review，降低 AI“错上加错”的风险。
- SDD 不是消除 AI 不确定性，而是将其引导至可预测、可验证、可干预的轨道。

## 提炼

> 一句话定位：以 SDD 规范让 Claude 统筹、Codex 编码、Gemini 分析交付

### 分节提炼

- **01 理解与规划**：Claude 先澄清业务目标，如“支持德国 GDPR 数据删除”。；Codex 负责细化技术方案与接口设计。；涉及多国法规时调用 Gemini 获取全局合规视图。
- **02 实现与运行**：非平凡代码变更先向 Codex 索要 unified diff 原型。；Claude 手动审查、改进后再应用最终代码，保障可控。
- **03 审查与分析**：Codex 负责代码与设计审查。；Gemini 分析测试日志、发现异常模式并建议消融实验。；结论冲突时让两模型互辩，由 Claude 仲裁。
- **04 撰写**：Gemini 总结法规要点、生成用户条款。；Codex 校验代码片段与文档一致性。；Gemini 默认只读，实现与最终决策仍由 Claude 完成。
- **05 强制规则检查**：CLAUDE.md 是强制规则而非建议。；Tool usage is the default，跳过工具必须简要说明原因。
- **06 决策提醒机制**：每个关键节点提醒 Claude 自问 Codex/Gemini 能否帮忙。；把工具调用设为默认行为而非可选项。
- **07 职责清晰**：Claude 协调者必须做决策。；Codex 代码专家、Gemini 分析专家，边界明确。；清晰边界避免重复工作。
- **08 工作流程标准化**：标准化 4 步循环：理解→实现→分析→撰写。；用户只面对单一 Claude 界面，背后三位 AI 自动协同。
- **09 安装步骤**：先移除官方 Codex MCP，再安装社区版 CodexMCP。；用 claude mcp list 验证 Connected 状态。
- **10 配置提示词**：强制 Codex 只给 unified diff，严禁真实修改代码。；编码完成后必须立即用 Codex review 改动。；Codex 只是参考，Claude 需独立判断与质疑。
- **11 Gemini CLI 安装**：Gemini MCP 用一条 npx 命令安装并验证。；可选装 Yuque MCP 供 Claude 读取语雀 Spec-PRD。

### 带走三句话

1. 规范先行：先把需求写成 Spec 契约，AI 才有共享语境与约束边界。
2. 多模型分工要固化到配置：Claude 统筹决策、Codex 出代码原型、Gemini 只读分析。
3. 关键节点必须人工确认：分阶段暂停 Review，防止错误累积到千行代码才暴露。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
