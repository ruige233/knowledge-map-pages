---
title: "Agency Agents"
url: "https://github.com/msitarzewski/agency-agents"
source: "GitHub / msitarzewski"
published: "2025-10-13"
archived: "2026-10-25"
category: "SKILL"
tags: [Claude Code, AI 代理, Agent 角色, 多工具集成, 提示工程]
lang: en
status: ok
---

# Agency Agents

> 一句话定位：一个开源的「AI 代理机构」角色库，把 280+ 个带人格与交付标准的专家代理做成可直接装进 Claude Code 等编码工具的 Markdown 文件，并附原生桌面应用与多工具转换脚本。

## 摘要

The Agency 是一个源自 Reddit 讨论的开源 AI 代理角色库，README 名册现列 282 个专业代理、覆盖 18 个部门——从工程、设计、付费媒体、销售、市场，到安全、GIS、医疗、学术、游戏开发、空间计算等，远超其 Stats 区仍在宣称的「230+」。每个代理按「身份与个性、核心使命、领域关键规则、技术交付物、工作流、成功指标」统一骨架编写，强调可衡量的产出而非通用提示模板。Claude Code 与 GitHub Copilot 可直接复制 `.md` 使用；`convert.sh` + `install.sh` 能把同一批代理转成 Cursor、Windsurf、Aider、Codex、OpenClaw、Gemini CLI、Antigravity、Qwen、Kimi、Osaurus、Hermes、DeepSeek Harness、Mistral Vibe 等 16 种工具的格式，并支持按部门/按代理只装子集。项目另推原生桌面应用 Agency Agents（macOS/Linux/Windows，可 brew cask 安装），一键浏览名册并自动更新。MIT 许可、主语言 Shell；截至 2026-10-25 约 163.6k stars。

## 要点

- **规模与结构**：README 名册列 282 个代理、18 个部门（工程 65、Specialized 58、市场 37、游戏开发 21、GIS 13、安全 12 等），仓库累计上万行人格设定、流程与代码示例；Stats 区仍写「230+」，已滞后于实际名册。
- **统一的代理文件骨架**：Identity & Memory、Core Mission、Critical Rules、Technical Deliverables、Workflow、Success Metrics——可直接 fork 改写，这也是它区别于一次性提示词集合的地方。
- **多工具适配（16 种）**：Claude Code / Copilot 原生 `.md`；转换与安装脚本覆盖 Antigravity、Gemini CLI、OpenCode、Cursor、Aider、Windsurf、OpenClaw、Qwen Code、Kimi Code、Codex、Osaurus、Hermes、DeepSeek Harness（`--tool dsh`）、Mistral Vibe（`--tool vibe`）。
- **原生桌面应用**：agencyagents.app 支持 macOS / Linux / Windows，免 clone 浏览全部名册并一键安装到 Claude Code、Cursor、Codex、Gemini、Osaurus 等工具、自动更新；macOS 可 `brew install --cask msitarzewski/agency-agents/agency-agents`。
- **按需安装**：`--division`、`--agent`、`--agents-file` 可只装所需团队；OpenCode 运行时只注册约 119 个代理、其余被静默丢弃，需用子集安装规避。
- **热度与维护**：MIT 许可、主语言 Shell；仓库创建于 2025-10-13，最近推送 2026-10-19；API 快照（2026-10-25）约 163,649 stars、26,412 forks。

## 关联

- `SKILL/Anthropic-Skills.md`、`SKILL/Superpowers.md` — 同为可复用的能力包，可对照角色包的组织方式。
- `教程/Claude-Code-Best-Practice.md` — 讲清了 Subagents 的目录约定与配置方式，是本仓库的主要落地工具。
- `教程/Learn-Claude-Code.md` — 从 Harness 工程视角解释「工具 + 知识 + 权限」如何组装成一个 Agent 产品。

## 注意

- README 的 Stats 区仍宣称「230+ 代理」等旧数字，本条目以 README 实际名册（282 个、18 个部门）为准。
- `published` 填的是仓库创建日期（2025-10-13），项目本身没有正式发布日期。
- API 返回的 `topics` 为空，标签取自 README 与项目定位；stars/forks 为 2026-10-25 抓取快照，会随时间变化。
