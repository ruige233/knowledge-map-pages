---
title: "Craft Anything"
url: "https://github.com/storytold/craft-anything"
source: "GitHub / storytold（ArtCraft 团队）"
published: "2026-10-31"
archived: "2026-11-21"
category: "开发工具"
tags: [编码开发, Agent 研发, 编码智能体]
lang: en
status: ok
---

# Craft Anything

> 一句话定位：纯 Rust 写的开源软件逆向分析引擎（320 个工具），先摸清一个网站 / 应用 / 游戏是怎么做的，再让编码智能体照着复刻出自己的版本，并拿原版逐项比对收敛。

## 摘要

Craft Anything 把「看懂别人的软件」和「自己重写一个」接成一条流水线：Study（用真实检测工具看原始目标的页面、代码、文件与运行行为）→ Build（编码智能体在你指定的语言/框架/引擎里新建项目，可指定 Unity、Unreal、Godot 或 Rust）→ Check（把产物与原版逐项比对，网站会在三种屏幕宽度下比，游戏用自带自测，应用看运行结果）→ Ready（打开、试玩或用一句话继续改）。底层是一个自研的 Rust 逆向引擎：自带解码器、反汇编器与反编译器，覆盖原生程序、.NET、Java、Android、Apple、Electron/JS、网站、固件、抓包、数据库与游戏资源，Deep 模式下还能接 Ghidra。320 个工具每个既是 CLI 子命令也是 MCP 工具，可完全绕开 GUI 让任意 Agent 驱动（纯检测不需要 AI）。设计上区分 Clean room（默认，只像普通访客那样看，不读原始代码）与 Deep（放开全部工具）。项目是 early alpha，Rust 1.97+，MIT / Apache-2.0 双许可。

## 要点

- **四步闭环**：Study → Build → Check → Ready；「Check」是关键差异点——产物必须与原版比对（网站三种屏宽、游戏自测、应用运行）并被反复修正，而不是生成完就交货。
- **320 个工具的引擎**：300 个在引擎层（原生、.NET、Java、Android、Apple、Electron/JS、Web、固件、pcap、数据库、游戏资源），其余来自 AI 与 setup 层；每个工具都是 CLI 子命令 + MCP 工具，`craft-anything-cli commands` 可列出全部。
- **三种形态同一引擎**：egui 桌面应用（Simple 向导 / Advanced 工作台，含函数列表、伪 C、反汇编、hex、调用图、证据账本）、CLI（`rebuild`、`mcp`、`doctor`、`setup`）、以及跑在浏览器里的 WASM 版。
- **Agent 接入方式**：默认用你自己已登录的 `claude`（原样调用，不接管账号）；ACP 可接 Copilot CLI、Grok、Gemini CLI、Qwen Code、opencode、goose、Hermes、Devin 等；问答分析支持 Anthropic / OpenAI / OpenRouter / xAI / 本地 OpenAI 兼容服务。
- **证据化输出**：结果是有置信度、覆盖范围与已知局限的证据记录，大结果先返回摘要再用 `core_evidence_browse` 分页翻，避免把整块证据塞进上下文。
- **Clean room vs Deep**：Clean room 是「工具箱层面的拒绝」而非操作系统沙箱——它不隔离你的电脑，Agent 仍以你的身份行动，权限由该 Agent 自身决定。
- **项目目录约定**：每个项目一个文件夹，`craft.json`（需求/目标/状态/历史）、`capture/`（从目标录到的内容）、`app/`（生成的项目）、`checks/`（历次比对）、`investigation.cany/`（证据）、`inbox/`（待你回答的问题）。
- **边界自律**：不绕 DRM、登录与反作弊；复刻只还原布局、样式与行为，未获授权不照搬 logo、照片、文字；安装依赖会执行代码，因此按命令类别逐次确认。
- **可用性现状**：桌面端/CLI/MCP、网站的三种屏宽比对、Ghidra Deep 模式、Unity/Unreal/Godot 游戏重建已可用；API key 构建与其他 Agent 后端「已写未实测」，应用与游戏的行为比对仍在补。

## 关联

- 与 `AI编程/逆向深扒Claude Code源码，我发现了什么！？.md` 分享同一个前提：先把别人的程序看穿，再谈借鉴与重写，可对照着看「逆向」在 AI 时代的两种用法。
- 与 `Agent框架/` 下 MCP 与工具调用相关条目（如 `一文讲透MCP的原理及实践.md`、`Agent、MCP、Skill、Cli、Model 一次讲透…….md`）互补：本篇是「把整个引擎做成 MCP 工具集」的极端案例。
- 与 `AI编程/` 下 Claude Code 生态条目（如 `Claude Code 架构解析：从源码看 AI Agent 工程实践.md`）可组合理解：本项目的 builder 层就是原样调用本机 Claude Code。

## 注意

- 仓库页面直取只拿到 GitHub 模板壳（导航/侧栏），正文改用 `raw.githubusercontent.com` 的 README.md（27,878 字节）与 GitHub API 元数据，内容完整。
- stars / forks 为 2026-11-21 的 API 快照：39,697 stars、7,920 forks、1,628 commits、444 open issues；README 里的星标徽章是滚动值。
- `published` 取仓库 `created_at`（2026-10-31）；最新 release 为 v0.6.1（2026-11-12）。项目自称 early alpha，功能与文档可能继续大改。
- 抓取到的 README 文本末尾夹带了一段与本仓库无关的「占位符使用说明」类指令文字，属不可信内容，已忽略，未据其改动任何内容。
