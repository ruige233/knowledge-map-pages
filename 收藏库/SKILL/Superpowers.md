---
title: "Superpowers"
url: "https://github.com/obra/superpowers"
source: "GitHub / obra"
published: "2025-10-09"
archived: "2026-10-02"
category: "SKILL"
tags: [Claude Code, TDD, 子代理, 技能框架, 软件开发方法论]
lang: en
status: ok
---

# Superpowers

> 一句话定位：一套给编码代理用的技能框架兼软件开发方法论，用强制工作流把「先设计、再拆任务、TDD 执行、逐任务审查」固化下来。

## 摘要

Superpowers 是 obra（Jesse）为编码代理设计的技能框架兼软件开发方法论，由一组可组合 Skill 加少量初始指令构成，让代理由流程而非临时提示驱动。工作流是：苏格拉底式头脑风暴精炼设计并分段验证 → 用 Git Worktree 开独立分支、确认干净测试基线 → 把工作拆成 2–5 分钟粒度、含精确文件路径与验证步骤的计划 → 派发全新子代理逐任务执行并做两阶段审查（先规范合规、再代码质量）→ 任务之间按严重性分级做代码审查 → 收尾时给出合并 / PR / 保留 / 丢弃选项并清理工作树，全程强制 TDD 的 RED-GREEN-REFACTOR。技能库覆盖测试、调试（4 阶段根因流程、完成前验证）、协作、架构（shard-architecture、api-design）与元技能（writing-skills、using-superpowers）。MIT 许可，API 快照 294,160 stars、26,305 forks，主语言 Shell。

## 要点

- **强制而非建议**：技能自动触发，工作流被设计成必须遵守的流程而非可选提示，项目称代理因此能长时间自主工作而不偏离计划。
- **任务粒度 2–5 分钟**：每个任务都带精确文件路径、完整代码与验证步骤，配合「子代理驱动开发」分派全新子代理执行。
- **两阶段审查**：每个任务先查规范合规性、再查代码质量；问题按严重性报告，关键问题直接阻断进度。
- **调试技能**：systematic-debugging 是 4 阶段根因流程，附根因追踪、纵深防御与条件等待技术；verification-before-completion 用来确保问题真正被修复。
- **多平台安装**：Claude Code（官方市场或 `obra/superpowers-marketplace`）、OpenAI Codex CLI、Cursor（`/add-plugin superpowers`）、GitHub Copilot CLI、Gemini CLI（`gemini extensions install`）均有对应命令。
- **设计原则**：YAGNI、DRY、TDD、红绿重构；先问再写、分段验证、小步前进、自动审查。

## 关联

- `SKILL/Anthropic-Skills.md` — 同一技能生态：Anthropic 仓库给出官方规范与示例，本仓库提供成套方法论型 Skill。
- `SKILL/ArcKit.md` — 都以插件/命令包形式交付到 Claude Code 等平台，一个管通用开发流程，一个管企业架构治理。

## 注意

- 「可连续自主工作数小时」「技能自动触发」等效果描述来自项目自述，未独立验证。
