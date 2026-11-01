---
title: "Free Claude Code"
url: "https://github.com/Alishahryar1/free-claude-code"
source: "GitHub / Alishahryar1"
published: "2026-01-28"
archived: "2026-10-02"
category: "AI编程"
tags: [Claude Code, 代理服务器, 多模型路由, NVIDIA NIM, OpenCode]
lang: en
status: ok
---

# Free Claude Code

> 一句话定位：本地代理服务器，把 Claude Code 等编码 CLI 的请求路由到免费或低价模型上，并按模型档位分流、在故障时自动换模型。

## 摘要

一个轻量级代理服务器，把 Claude Code 的 API 调用路由到免费或低成本模型，源笔记列出的后端包括 NVIDIA NIM、OpenRouter、DeepSeek、LM Studio、llama.cpp，并可为 Opus、Sonnet、Haiku 不同档位分别指定后端。它还内置配额探测、标题生成等本地拦截优化，无需调用提供商即可省配额和延迟，并支持 Discord、Telegram 机器人远程控制编程以及 VS Code、IntelliJ 插件配置。仓库 README 的现状比源笔记更大：自称 59 个遵守服务条款的提供商、每月 13 亿以上免费 token，可驱动 Claude Code、Codex、Pi、OpenCode、Cline、Aider 等 11 种编码 agent，并支持浏览器内 Codex 会话与 Whisper 语音输入。Python 3.14 实现。

## 要点

- **分级路由**：Claude Code 的 Fable/Opus/Sonnet/Haiku 档位可各指定模型，`MODEL` 作为兜底，另可配有序 Fallback Models 列表——请求失败自动切换模型而不中断本轮。
- **本地拦截省配额**：配额探测、命令前缀识别、标题、建议、文件路径等 5 项优化不调用提供商；可选 RTK 过滤常见命令输出，README 称最多减少 90% 终端输出 token。
- **覆盖面（README 自述）**：59 个提供商、11 种编码 agent（Claude Code、Codex、Pi、OpenCode、Cline、Hermes、DeepSeek Harness、Grok Build、Muse Code、Aider、VS Code Chat）、每月 13 亿以上免费 token。
- **接入形态**：Admin UI 配置提供商与模型，终端启动器（`fcc-claude`、`fcc-codex`、`fcc-opencode` 等）、桌面端、VS Code / JetBrains 集成、Discord 与 Telegram 机器人、浏览器内 Codex 会话，语音输入可用 NVIDIA NIM 或本地 Whisper。
- **关注度**：2026-10-02 API 快照 56,382 stars、9,018 forks；主语言 Python，创建于 2026-01-28，最后推送 2026-10-02。

## 关联

- `AI编程/Everything-OpenCode.md` — 同批条目，都扩展 OpenCode / Claude Code 这类编码 CLI：本条解决模型来源与配额，那条补 agent、命令与 skill 配置。
- `教程/Claude-Code-Best-Practice.md` — 与 Claude Code 的成本控制和使用实践相关。

## 注意

- 源笔记（2026-04-26）只列了 NVIDIA NIM、OpenRouter、DeepSeek、LM Studio、llama.cpp 及 Discord/Telegram/IDE 集成；59 提供商、11 种 agent、13 亿 token、90% 输出节省等数字来自 2026-10-02 抓取的仓库 README（项目方自述），未独立验证。
- 许可证信息不一致：GitHub API 返回 `NOASSERTION`，README 徽章标注 AGPL-3.0-only。
- 源笔记标注 Star 数"1 万+"，API 快照为 56,382，两者为不同时点数据，本条目按 API 记录。
