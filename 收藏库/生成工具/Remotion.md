---
title: "Remotion"
url: "https://github.com/remotion-dev/remotion"
source: "GitHub / remotion-dev"
published: ""
archived: "2026-10-30"
category: "生成工具"
tags: [React, 视频生成, 程序化视频, Agent Skills]
lang: en
status: ok
---

# Remotion

> 一句话定位：用 React 写代码来生成视频的开源框架——同一套视频代码既可以交给 Agent 自动产出，也能在编辑器里交互式剪辑，还能在自有基础设施上批量渲染。

## 摘要

Remotion 是 remotion-dev 开源的"程序化视频"框架，核心是把视频当作 React 代码来写：画面、动画、转场、字幕、字体、色彩校正等都用 Web 技术（React 组件 + CSS 动画）表达，视频以代码为唯一事实源，天然支持版本控制、组件复用与批量变体。官方给出三种使用方式：让编码智能体把想法变成视频（agentically）、像常规剪辑器一样交互式编辑动画（Studio）、以及完全用代码掌控视频（programmatically）。渲染层支持 Node.js APIs、AWS Lambda、Vercel 与客户端渲染，适合设计系统、百万级批量渲染和嵌入式视频应用。上手命令为 `npx create-video@latest`，文档规模超 1200 页。需注意其许可特殊，部分商用场景需购买公司许可。

## 要点

- **三种用法覆盖不同人群**：Agent 驱动（把想法交给智能体生成视频）、交互式编辑（Remotion Studio 内像剪辑器一样动效）、纯代码（React + Web 技术，视频即代码）。
- **视频即代码**：画面与动效由 React 组件和 CSS 动画定义，改一处即可批量重渲染出无数变体，适合设计系统与自动化流水线。
- **渲染选项多样**：Node.js APIs、AWS Lambda、Vercel、客户端渲染均有官方支持，可对比后按场景选择，支持在自有基础设施上渲染百万级视频。
- **上手极简**：`npx create-video@latest` 一键起步；官方文档超 1200 页，并提供 Agent Skills 文档（remotion.dev/docs/ai/skills）。
- **许可特殊**：并非标准 MIT，部分情况下需购买公司许可，商用前需阅读 LICENSE.md 确认。

## 关联

- 与 `收藏库/生成工具/Hypit.md` 互补：Hypit 是面向编码智能体的"视频编程"工作流，Remotion 则是程序化视频的底层框架/运行时，思路同源。

## 注意

GitHub API 限流（HTTP 403），stars、license 类型、created_at/pushed_at 等仓库元数据未取到，故相关字段留空、未臆造；正文经 raw README 抓取成功。
