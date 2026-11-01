---
title: "Hypit"
url: "https://github.com/hypit-ai/hypit"
source: "GitHub / hypit-ai"
published: "2026-07-29"
archived: "2026-10-15"
category: "生成工具"
tags: [AI视频生成, SVML, Agent Skill, Claude Code, 短视频生成]
lang: en
status: ok
---

# Hypit

> 一句话定位：给编码智能体（Claude Code、Codex 等）补上一套"视频编程"能力——投一条参考视频，让 Agent 克隆出可编辑、可重跑的完整合成工作流，而不是一份分镜脚本。

## 摘要

Hypit 用自研标记语言 SVML 把画面、字幕、B-roll 与特效锚定到**词**而不是秒，因此改一句文案，时序会自动重排。用法是投一条参考视频，让 Agent 把整条工作流克隆下来；也可从模板起步，或直接描述需求让它现写。一条工作流能批量产出上百个变体：换主播、换钩子、换产品、换语言、换画幅。生成模型是可选的——字幕、动态图形与纯代码渲染的画面能在本地编译成片，不调用生成模型也就没有服务费。仓库自带 UGC 足球榜单、播客、街头采访三套示例，单条成本约 1.07–1.15 美元，以 64 个无头 Chromium 进程并发渲染，并集成 WhisperX 词级对齐与 YOLOv8 AnimeFace 头部追踪字幕。TypeScript 实现，pnpm Monorepo，Node 22.15+，许可为附条件的 Hypit Open Source License；截至 2026-10-15 约 22,365 stars / 2,492 forks，最新版本 v0.3.1。

## 要点

- **SVML 词锚定机制**：以词而非秒作为时间锚点，改写文案后字幕、特效与画面时序自动跟随；SVML 编译器把一份源码编译成字幕、动态图形、特效与代码渲染画面，而非单纯的时间轴数据。
- **安装与调用**：`npx skills add hypit-ai/hypit -g` 安装 Skill，首次使用时 Agent 会自检 Hypit 可执行文件并按需协助准备，视频工程目录可放在任何位置；在任意项目目录用 `/hypit` 触发，既支持"克隆这条参考视频并替换某部分内容"，也支持不给参考视频、只描述想要的片子让 Agent 现写工作流。
- **克隆粒度与变体生产**：投视频得到的是完整工作流（素材、字幕、B-roll、特效），复用同一构图与既有素材、只重新生成变化部分，因此能"一条工作流 100 个变体"；组件可插拔，换主播不必动字幕。
- **生成模型可选、无附加费**：不接生成模型也能靠字幕 + 动态图形 + 代码渲染出片；接模型时可用推荐托管服务 HypiHub，也支持自备 API 或本地模型。Hypit 本身不收席位费、按渲染收费，也不加水印，模型服务费用由所选服务商结算。README 另列出 Launch Partners（Monid、HiAPI、Pollo API、BeatAPI、TokenDance 等多模型网关，以及 Watcha、AgentSky、AutoClaw、OpenAgents Workspace 等 Agent 运行环境），属商业合作位，价格与可用性需自行核对。
- **示例与成本**：UGC 足球榜单 "GOAT DEBATE"（20 秒）$1.15、播客 "DAILY CREATINE"（18 秒）$1.07、街头采访 "NICE RIDE"（26 秒）$1.09，每套各附 3 条克隆变体；素材链混用 Seedance 2 Mini 720p、GPT Image 2、WhisperX 词级对齐、Sound-synced 榜单板、颜色框卡拉 OK 字幕，头部追踪字幕由 Google Video Intelligence 人脸框 + YOLOv8 AnimeFace 驱动，渲染用 64 个无头 Chromium 进程并发。
- **目标场景与工程现状**：付费社媒广告钩子批量测试（克隆 Meta 广告库里的赢家）、爆款克隆、TikTok Shop 与联盟带货、AI UGC 口播、播客/访谈切片、纯代码渲染视频、多语言本地化。最新 release 为 v0.3.1（2026-10-09），并提供 0.3 / 0.3.1 SDK 迁移指南；约 1,493 次提交，最近提交 2026-10-14；官方 Quickstart 与开发指南在 hypit.ai，仓库另有简体中文 `README.zh-CN.md`；组件可打包成版本化 tarball 或发布到自有 npm scope，进入官方 Distribution 需先开 issue 说明共享产品需求。数据快照：2026-10-15 约 22,365 stars / 2,492 forks / 16 open issues，主语言 TypeScript，采用 pnpm Monorepo。

## 关联

- 与同分类的 [MoneyPrinterTurbo](MoneyPrinterTurbo.md) 目标相同但路径相反：后者由主题关键词驱动全自动流水线（文案 → 素材 → 配音 → 字幕 → 成片），Hypit 由参考视频反推可复用工程，且把生成模型降级为可选项。
- 与同分类的 [Open-Generative-AI](Open-Generative-AI.md) 互补：后者是模型超市式生成工作室（600+ 模型、文生图/视频、唇形同步），Hypit 更偏"合成与批量化"的编排层，可直接调用视频/图像/语音模型。
- 以 `npx skills add` 分发的 Agent Skill，分发型态与 [../SKILL/Anthropic-Skills.md](../SKILL/Anthropic-Skills.md)、[../SKILL/Superpowers.md](../SKILL/Superpowers.md) 同类，但本条目按其核心用途（视频生成）归入生成工具。

## 注意

- 星数与 README 徽章（含 Trendshift 日榜第 1 徽章）均为滚动值；本条两次快照数据取自 2026-10-12 与 2026-10-15 的 GitHub API（21,521 → 22,365 stars，2,364 → 2,492 forks）。
- `published` 采用仓库创建日期 2026-07-29。已核实 main 分支最早提交即 "Initial commit"（`ffa519d`，2026-07-29T18:49:19Z，与 `created_at` 同秒），因此上一版"最早可见提交为 2026-10-09、提交历史疑似重写"的判断已更正——那次很可能只查了某个具体路径的提交。未采用 `pushed_at`（2026-10-15，属最后活动时间而非发布日）。
- 许可证并非纯 Apache-2.0：GitHub API 记为 Other / NOASSERTION，README 徽章标注 "Apache-2.0 with conditions"，实际文本为仓库内 `LICENSE`（Hypit Open Source License）；商用前需读原文。README 的 License 一节明确产出归使用者所有，第三方模型与服务另有条款。
- README 中"Clone any viral video""$1.15 成本"以及各 Launch Partner 的能力描述均为项目方与合作伙伴的自述，成本取决于所选模型服务与用量，未独立验证；README 里"免费领取 100 个 AI 数字人"指向第三方 Google Drive 链接，同样未验证。

