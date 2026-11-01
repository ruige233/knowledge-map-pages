---
title: "Open Generative AI"
url: "https://github.com/Anil-matcha/Open-Generative-AI"
source: "GitHub / Anil-matcha"
published: "2023-05-09"
archived: "2026-10-02"
category: "生成工具"
tags: [图像生成, 视频生成, 唇形同步, 本地推理, Flux]
lang: en
status: ok
---

# Open Generative AI

> 一句话定位：无内容过滤的开源 AI 图像与视频生成工作室，一个界面里切换数百个模型，可自托管、也可用官方 Web 版和桌面客户端。

## 摘要

Open Generative AI 面向创意工作者，主打"不受限 + 模型超市"两点。它把文生图、图生图、文生视频、图生视频和唇形同步（lipsync）这几类生成能力拆成多个分工明确的工作室，用户在同一套界面里调用包括 Flux、Kling、Sora、Veo 等在内的主流模型，并明确宣称不做提示词内容过滤与拒绝。除云端模型外，它也支持本地推理，可跑 Z-Image Turbo、Dreamshaper、SDXL 等开放权重模型，因此素材不必离开本机。交付形态上有三条：官方 Web 版可直接使用；自托管部署（MIT 许可，主语言 JavaScript）；以及 macOS、Windows、Linux 桌面客户端，其中在 Apple Silicon 上可利用 Metal GPU 加速。需要留意的是仓库自述的模型数量已随迭代增长到 600+，明显高于早期笔记记录的 200+；热度也远超来源笔记的 8000+，2026-10-02 快照约 29.5k stars、5.4k forks。

## 要点

- **能力矩阵**：文生图、图生图、文生视频、图生视频、唇形同步，按用途拆分为多个独立工作室。
- **模型规模随版本增长**：仓库当前描述为 600+ 模型（早期笔记记 200+），涵盖 Flux、Midjourney、Kling、Sora、Veo、Wan、Seedance2 等。
- **无内容过滤**：官方明确不做内容过滤器与 prompt 拒绝，这是它与主流托管平台的定位差异。
- **本地推理可选项**：支持 Z-Image Turbo、Dreamshaper、SDXL 等模型本地跑，配合自托管实现素材不出本机。
- **多端交付**：Web 版 + macOS / Windows / Linux 桌面客户端；Apple Silicon 可用 Metal GPU 加速。
- **关注度数据**：2026-10-02 快照约 29.5k stars / 5.4k forks，MIT 许可，仓库创建于 2023-05-09。

## 关联

- 与同分类的 [MoneyPrinterTurbo](MoneyPrinterTurbo.md) 形成互补：后者用现有素材与配音拼装成片，本条目直接生成画面与视频。
- 与 [MarkItDown](MarkItDown.md) 同属多模态内容处理工具链。

## 注意

- 仓库自述模型数为 600+，来源笔记记为 200+；本条以 GitHub 仓库当前描述为准，两个数字反映不同时间点的版本状态。
- 来源笔记标注 star 数为 8000+，与 2026-10-02 API 快照（29.5k）差距较大，应为笔记撰写时的旧值。
- 来源笔记未给出许可证与主语言，本条的 MIT 与 JavaScript 取自 GitHub API。
