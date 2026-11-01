---
title: "BaoCut"
url: "https://github.com/JimLiu/baocut"
source: "JimLiu (GitHub)"
published: "2026-07-13"
archived: "2026-10-25"
category: "生成工具"
tags: [AI视频, 视频剪辑, MCP, Claude Code, 智能体]
lang: en
status: ok
---

# BaoCut

> 一句话定位：以“可编辑时间线”为核心的 AI 视频智能体，把转写、字幕、翻译、配音、剪辑与动效都沉淀进同一个可继续编辑的视频工程。

## 摘要

BaoCut 是围绕“可编辑视频”构建的 AI 视频智能体（Electron 桌面应用，macOS/Windows），由同一个本地 Runtime 驱动桌面端、CLI、MCP 服务与 Web 客户端四种入口。核心理念是“视频即唯一事实源”：转写、字幕、翻译、章节、配音和动效都作为时间线内的图层存在，SRT、MP4、音频只是导出结果，局部修改不会触发整片重渲染。它支持本地模型（Whisper、Qwen3-ASR 等）与在线 API 做词级转写、字幕翻译、人声分离配音和去语气词剪辑；也能从一句话 brief 用 24 个场景模板生成视频，并让智能体以“代码即动效”的方式编写 HTML 合成并烘焙成帧。智能体在显式范围、访问模式与预算内工作，破坏性操作需先确认；Codex CLI 与 Claude Code 已端到端验证，任何 ACP 智能体都可接入。仓库为 TypeScript 主语言，自定义许可证（Other），截至 2026-10-25 约 1420 star。

## 要点

- 以可编辑时间线为源：所有内容都是图层，文件只是导出；局部编辑（改一行字幕、换一个镜头、重配一句配音）只改该处并给出变更回执，不整体重渲染。
- 核心能力：词级时间戳转写、原语/翻译/双语字幕、HTDemucs 人声分离加逐句配音、去语气词与停顿的剪辑、章节/摘要/博客/短片等派生内容、从 prompt 生成视频（24 个内置场景模板）、HTML 动效合成（支持 `baocut/1` 与 `hyperframes/1` 合约）。
- 四种入口共用同一 Runtime：Electron 桌面端、`baocut` CLI、MCP 服务 + BaoCut skill（`baocut skill install` / `baocut mcp install`）、Web 客户端（一次性链接远程查看）。
- 引擎与模型解耦：验证过 Claude Code、Codex CLI，内置 GitHub Copilot CLI、Pi、OpenCode，任意 ACP 智能体可接；能力可指向本地模型（MLX/Core ML、whisper.cpp+candle）或 OpenAI、Anthropic、DeepSeek、Qwen、Moonshot 等在线端点，并按任务记账。
- 运行要求：桌面安装包自带 Runtime 与原生 worker；智能体功能需已登录的 Codex CLI/Claude Code 等引擎，媒体分析、转写准备与导出需自行安装 ffmpeg 与 ffprobe。
- 源码开发需 Node.js 24.22.1+ LTS、Rust、CMake 等（`dev:lite` 可跳过 Rust 构建但无原生能力）；macOS Apple Silicon 与 Windows x64（CPU/CUDA/Vulkan）均提供安装包。

## 关联

- 与 `生成工具/MoneyPrinterTurbo.md` 同属 AI 视频生成方向：MoneyPrinterTurbo 是批量短视频流水线，BaoCut 强调可编辑时间线与智能体交互。

## 注意

- star 数为滚动值，本条目以 2026-10-25 的 1420 为参考快照。
- 许可证为自定义（GitHub 标注 “Other”），商用前需查看仓库 LICENSE 原文。
