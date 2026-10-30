---
title: "Everything OpenCode"
url: "https://github.com/jakezp/everything-opencode"
source: "GitHub / jakezp"
published: "2026-01-30"
archived: "2026-10-02"
category: "AI编程"
tags: [OpenCode, Agent, Skills, TDD, LSP]
lang: en
status: ok
---

# Everything OpenCode

> 一句话定位：OpenCode CLI 的一站式配置套件，把 everything-claude-code 的 agent、命令、skill、规则与 hooks 移植过来，装完即可用。

## 摘要

OpenCode CLI 的完整配置套件，源自 everything-claude-code 并针对 OpenCode 适配优化，一次性提供 16 个专业 agent、25 个斜杠命令、24 个 skills、9 套规则集，以及 pre/post-tool-use、会话生命周期、压缩提示等 hooks。Agent 覆盖规划与架构（planner、architect、orchestrator）、多种审查（code-reviewer、security-reviewer、database-reviewer、go-reviewer）、TDD 与 E2E、构建错误修复、重构清理、文档更新、前端设计以及截图/PDF 视觉分析（multimodal-looker）；skills 含 coding-standards、tdd-workflow、continuous-learning、backend/frontend-patterns、golang-patterns、lsp-validation。安装后组件复制到 `~/.config/opencode/`，模型可用预设或按 agent 混合配置，还可选 tmux 后台任务与 LSP 工具。MIT 许可，主语言 TypeScript。

## 要点

- **组件规模**：16 个 agent、25 个斜杠命令、24 个 skills、9 套规则（安全、编码风格、测试、Git 工作流、LSP 验证），来源标注为 everything-claude-code 的 OpenCode 版。
- **安装接入**：`npm install -g everything-opencode` 后执行 `everything-opencode install` 复制组件到 `~/.config/opencode/`，并在 `opencode.json` 的 `plugin` 中加入 `everything-opencode`。
- **模型分层配置**：预设 `github-copilot` / `anthropic` / `bedrock` / `google` / `openai`，支持按 agent 混合提供商（如 designer 用 Gemini、multimodal-looker 用 Gemini Flash），也可用 `EVERYTHING_OPENCODE_PRESET` 环境变量或项目级 `.opencode/everything-opencode.json` 覆盖。
- **工程化 hook**：tmux 长任务提醒、阻止在 tmux 外启动开发服务器、阻止创建不必要的 markdown、Git push 提醒、编辑后 console.log 警告，以及每 20 次编辑给出战略压缩建议。
- **附加能力**：LSP 工具（跳转定义、查找引用、诊断、重命名）与可选 tmux 并行会话管理，来源标注为 oh-my-opencode-custom。
- **关注度**：2026-10-02 API 快照 19 stars、1 fork；MIT 许可，主语言 TypeScript，创建与最后推送同为 2026-01-30。

## 关联

- `SKILL/Anthropic-Skills.md`、`SKILL/Superpowers.md` — 同为面向编码智能体的 skill/agent 资产包，可对比其组织方式。
- `教程/Claude-Code-Best-Practice.md` — 上游 everything-claude-code 属于 Claude Code 配置与工作流实践，本条是其 OpenCode 分支。
- `AI编程/Free-Claude-Code.md` — 同批条目，都围绕 OpenCode、Claude Code 等编码 CLI 做扩展，本条补 agent/命令/skill，那条补模型与配额。

## 注意

- 仓库无 description 也无 topics，API 除许可证与语言外未提供可用说明，功能清单以源笔记为准。
- 关注度很低（19 stars / 1 fork），且 API 显示最后推送时间与创建时间同为 2026-01-30，之后无提交记录；源笔记标注的版本 0.1.7 未在 API 中体现。
- 源笔记中的模型名（claude-opus-4-5、gemini-3-pro-high、gpt-4o 等）为项目配置示例，未核验其当前可用性。
