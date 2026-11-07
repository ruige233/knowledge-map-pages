---
title: "Taste Skill"
url: "https://github.com/Leonxlnx/taste-skill"
source: "GitHub · Leonxlnx"
published: "2026-02-19"
archived: "2026-11-03"
category: "SKILL"
tags: [设计与人机交互, Agent Skills, 编码智能体]
lang: en
status: ok
---

# Taste Skill

> 一句话定位：给编码智能体注入"设计品味"的 Agent Skill，让 AI 产出有美术指导、有真实素材、经过真实浏览器自检的网站与应用界面，而不是千篇一律的 AI 味落地页。

## 摘要

Taste Skill（安装名 `design-taste-frontend`）是一个面向编码智能体的 Agent Skill，目标是让 Claude Code、Codex 等工具在设计网页和 App 界面时产出有审美取向的作品，而非平均化的 AI 页面。v2 版本基于对 247 个精选站点的研究，构建流程分六步：读简报列出客户独有信息、从 20 个研究方向中发牌选出 3 个供智能体挑选、用一句话把客户素材与方向绑定成创意、用开放档案中的真实图片与影像搭建页面并署名、在真实浏览器中以三种宽度检查并淘汰"AI 味"、最后看自己的截图修复最严重的三个问题（最多三轮）。v2.1 扩展了仪表盘与 App 界面的独立路线，用应用规则检查面板安静度、数字对齐、颜色使用和移动端导航，并检查页面框架（标题、边距、空白）。安装只需 Node 22+ 与 Chrome/Edge/Chromium，无需 API key。MIT 协议，官网 tasteskill.dev，另有桌面应用 TasteCode。

## 要点

- 安装：`npx skills add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend"`；也支持 Claude Code 插件（`/plugin install taste-skill@taste-skill`）和 Codex 插件，仓库还提供 `./install.sh` 符号链接安装方式。
- v2 六步流程：简报分析 → 从 20 个方向发 3 张方向牌 → 一句话创意 → 真实素材搭建 → 真实浏览器三档宽度检查 → 截图自审并修复 top3 问题（最多 3 轮）；单次构建约 30–45 分钟、消耗几十万 token。
- 图片来自 Wikimedia Commons、Openverse、The Met 等开放档案，只选用 PD/CC0/CC BY/CC BY-SA 素材并强制在页面署名，替代没有版权的占位图。
- v2.1 为仪表盘/App 界面走独立路线：发一张从真实产品界面提炼的应用风格牌，按"安静面板、对齐数字、颜色花在数据上、手机端导航折叠"等规则构建与检查（`inspect.mjs --app`）；产品落地页会以同样方式把所展示的 App 真正搭出来。
- 仓库元数据（2026-11-03 快照）：约 109.3k stars、7.4k forks，JavaScript，MIT 协议；移动/改名后的旧技能在 taste-skill-classics、imagegen-skills 等仓库中继续维护。
- 依赖极简：Node 22+ 和一个已安装的 Chrome/Edge/Chromium，不装其他依赖、不需要 API key；找图片时需要网络。

## 关联

- [Anthropic-Skills](Anthropic-Skills.md)：Agent Skills 生态的官方基础仓库，可对照理解本技能的安装与分发机制。
- [Effective-HTML](Effective-HTML.md)：同为面向 Claude Code 的前端产出类技能，偏 HTML 线框与原型；Taste Skill 侧重设计品味与真实浏览器质检，二者互补。

## 注意

- README 中未出现 star 数字，star/forks 数据取自 GitHub API，是 2026-11-03 的滚动快照，会随时间变化。
- README 含较多赞助商推广区块（interfaces.dev、React Bits、IMG.LY、Kimi、Vercel 等），摘要与要点已剥离。
