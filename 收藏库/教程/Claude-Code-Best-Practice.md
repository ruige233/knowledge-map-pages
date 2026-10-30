---
title: "Claude Code Best Practice"
url: "https://github.com/shanraisshan/claude-code-best-practice"
source: "GitHub / shanraisshan"
published: "2025-10-31"
archived: "2026-10-02"
category: "教程"
tags: [Claude Code, Agentic Engineering, Subagents, MCP, 上下文工程]
lang: en
status: ok
---

# Claude Code Best Practice

> 一句话定位：一份把 Claude Code 从氛围编程（vibe coding）推进到智能体工程（agentic engineering）的系统化最佳实践指南，围绕 Subagents / Commands / Skills 三大组件展开。

## 摘要

这是一份围绕 Anthropic Claude Code 的最佳实践指南，主线是「from vibe coding to agentic engineering」。它把 Claude Code 的能力拆成三大组件：Subagents 在独立上下文中自主执行，可自定义工具、权限、模型与记忆；Commands 是注入现有上下文、由用户调用的提示模板；Skills 是可预加载、自动发现并支持渐进式披露的知识包。指南还整理了 Hooks、MCP Servers、Plugins、CLAUDE.md 记忆与基于 Git 的 Checkpointing 等机制，说明了 `settings.json` 的项目级与用户级配置层级，并从提示工程、上下文工程、智能体工程三个方面给出可操作的实践建议。仓库采用 MIT 许可，主语言 HTML，2026-10-02 约 6.7 万 stars。

## 要点

- **三大组件的分工与位置**：Subagents 放 `.claude/agents/<name>.md`（独立上下文、可配工具/权限/模型/记忆）；Commands 放 `.claude/commands/<name>.md`（注入现有上下文的提示模板）；Skills 放 `.claude/skills/<name>/SKILL.md`（自动发现、可预加载、渐进式披露）。
- **配置分三级**：项目级 `.claude/settings.json`、用户级 `~/.claude/settings.json`、项目专属 `~/.claude/projects/<project>/`，可分别控制权限、模型选择、输出样式、沙盒、快捷键、Fast Mode 与显示上下文/模型/成本的状态栏。
- **进阶路径**：先用 Skills 预加载常用知识、用 Commands 标准化工作流，再用 Subagents 承接复杂独立任务，最后靠 Hooks + MCP Servers + Git Worktrees 做自动化与并行开发。
- **云端与协作能力**：Ultraplan（云端起草计划、浏览器审查、远程或终端执行）、Claude Code Web（长时任务、PR 自动修复、并行会话）、Telegram/Discord/webhook 事件通道、Slack 提及执行、GitHub App 多智能体 PR 分析。
- **概念词表**：Agentic Engineering（智能体工程）、Context Engineering（上下文工程）、Vibe Coding（自然语言驱动的氛围编程）是全文的三条主线，适合作为检索关键词。
- **元数据**：MIT 许可，主语言 HTML；API 快照约 66,989 stars、6,698 forks，仓库创建于 2025-10-31。

## 关联

- `教程/Learn-Claude-Code.md` — 一份讲用法与最佳实践，一份逆向拆解 Claude Code 的 Harness 机制，可互补阅读。
- `教程/Easy-Vibe.md` — 阶段三同样覆盖 Claude Code、MCP、Skills、Agent Teams 的实操教程。
- `SKILL/Anthropic-Skills.md` — 技能包的组织与写法可与此处的 Skills 章节互参。

## 注意

- `published` 取仓库创建日期（2025-10-31），仓库没有版本发布记录，也没有正式发布日期。
- 功能清单来自本地笔记并结合 GitHub API 元数据整理；Claude Code 迭代很快，具体命令、参数与可用范围可能已变化，未逐条比对官方文档。
- stars/forks 为 2026-10-02 抓取快照。
