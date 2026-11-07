---
title: "Guizang Social Card Skill · 小红书图文 / 公众号封面对"
url: "https://github.com/ruige233/guizang-social-card-skill"
source: "GitHub · op7418（归藏）"
published: "2026-05-27"
archived: "2026-11-01"
category: "SKILL"
tags: [Claude Code, Codex, 图文卡片, 小红书, Live Photo]
lang: zh
status: ok
---

# Guizang Social Card Skill · 小红书图文 / 公众号封面对

> 一句话定位：一个给 Claude Code / Codex 等 Agent 环境用的图文卡片技能，把文章、文案、截图、照片或视频排成小红书图文、Live Photo 动态卡与公众号 21:9 + 1:1 封面对。

## 摘要

这是归藏（op7418）出品的 Agent Skills 技能包，让编码智能体把文章、文案、截图、产品笔记、字幕、照片或用户视频，自动排版成小红书 / Rednote 图文组图、Live Photo 动态卡，以及公众号 21:9 头图 + 1:1 分享卡封面对。技能内置电子杂志风（Editorial）与瑞士国际主义（Swiss）两套视觉系统，提供 28 个版式骨架、10 套主题色预设与 3 个画板尺寸，以单文件 HTML 表达版式、Playwright 渲染出 PNG，并配有 validate-social-deck.mjs 做溢出、字号、密度等 9 条规则的自动校验。核心思路是把「稳定版式」当产品价值：版式先选后改、主题色只许从预设中挑，不靠自由发挥来保证每组图都能用。AGPL-3.0 开源。

## 要点

- 定位：为 Agent 环境设计的图文卡技能，输入文章 / 文案 / 截图 / 视频等素材，输出小红书组图、Live Photo 卡与公众号封面对；与 guizang-ppt-skill 是姊妹项目（共享美学语言、独立维护）。
- 双视觉系统：Editorial（杂志风，适合叙事 / 生活方式 / 旅行）与 Swiss（网格 + 单一锚点色，适合产品测评 / 数据 / 方法论），共用同一 7 步工作流。
- 版式资产：28 个版式骨架（Editorial 16 + Swiss 12）、10 套主题预设（Editorial 6 + Swiss 4 锚点色）、3 个画板尺寸（1080×1440 / 2100×900 / 1080×1080）。
- 渲染链路：单文件 HTML + CSS Grid 表达版式，`node render.mjs` 用 Playwright 直接出 PNG，无前端构建链；`validate-social-deck.mjs` 基于真实 DOM 测量做 9 条规则校验（溢出、字号上限、4 横带密度、footer 碰撞等）。
- Live Photo：支持单视频、二三四宫格、三连拼图与长视频稀疏抽帧诊断；小红书按 5s、公众号文章内按 3s 规划，交付 JPG + MOV + .pvt 并提示手机端发布路径。
- 使用约束：主题色只能从 10 套预设选、版式只能从 28 个骨架选改、默认不自动跑 validator（先看图再问用户）；AGPL-3.0，衍生品须开源。

## 注意

- 本地址 `ruige233/guizang-social-card-skill` 是 `op7418/guizang-social-card-skill` 的 fork，README 内容与上游一致；上游仓库 star 数为滚动值（2026-11-01 快照约 7.9k），本 fork 为 0。
- `published` 取上游仓库创建日期 2026-05-27；fork 创建于 2026-10-28。
