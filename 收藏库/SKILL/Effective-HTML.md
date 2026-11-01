---
title: "Effective HTML"
url: "https://github.com/plannotator/effective-html"
source: "Plannotator"
published: "2026-06-09"
archived: "2026-10-18"
category: "SKILL"
tags: [Agent Skills, HTML, 线框图, 原型, Claude Code]
lang: en
status: ok
---

# Effective HTML

> 一句话定位：Plannotator 出品的编码智能体技能集，用 HTML 产出有用、自包含的可视化制品——从低保真线框图到可交互原型、计划与图表。

## 摘要

Plannotator 出品的编码智能体技能集，聚焦用 HTML 产出有用、自包含的可视化制品——从低保真线框图到可交互原型、计划与图表。核心理念是"fat artifacts + fat context"：现代模型能力足够强，与其堆砌大段文字，不如用 HTML 把意图可视化，让提示词直接指向承载上下文的制品、仓库与文件夹。仓库含 7 个可选技能：`html` 总路由、`big-pics-little-words` 大图少字解释、`design-artifact` 创意方向、`html-wireframe` 低保真布局、`html-prototype` 可交互原型、`html-plan` 计划/路线图、`html-diagram` 各类图表。设计哲学把创意自由与可靠性分离：视觉方向来自对话与项目本身，线框图刻意保持"未完成"以聚焦结构评审，原型只实现一条可信流程及其状态。所有制品要求响应式、可访问、自包含并经浏览器验证。支持 `npx skills add` 安装及 Claude Code / Codex 插件；灵感源自 Thariq Shihipar 的《The unreasonable effectiveness of HTML》。

## 要点

- **7 个技能各司其职**：`html` 处理宽泛请求并路由到专家技能；其余分别负责大图少字解释、创意方向、低保真线框图、可交互原型、计划/路线图、各类图表（架构/时序/流程/状态/层级/时间线/系统图）。
- **核心理念 "fat artifacts + fat context"**：HTML 几乎能可视化任何东西，常比一堵文字墙更清晰；提示词只需指向承载上下文的制品与仓库，即可把仓库当参考资料用而无需安装。
- **创意自由与可靠性分离**：视觉方向来自对话、项目、受众与主题；`design-artifact` 提供可复用的设计流程但不强加固定视觉风格；线框图刻意保持未完成以聚焦结构，原型只实现一条可信流程。
- **统一的制品质量要求**：每个制品都要响应式、可访问、自包含，并在浏览器中验证。
- **安装方式**：`npx skills add plannotator/effective-html`（可用 `--list` / `--skill <名>` 单装）；Claude Code 用 `/plugin marketplace add plannotator/effective-html`，Codex 用 `codex plugin marketplace add ...`。
- **关联工具**：配套 Plannotator（本地渲染与标注 HTML）与 tot（为 HTML 文件生成可分享链接）。

## 关联

- 同属面向编码智能体、以 HTML 为交付物的可复用技能包，可与 `SKILL/Frontend-Slides.md`（HTML 网页演示文稿）对照阅读。
- 与 `SKILL/Anthropic-Skills.md`、`SKILL/Superpowers.md` 等同属 Agent Skills 集合，可用 `npx skills add` 安装。

## 注意

- GitHub 页面 `web_fetch` 只返回导航外壳，摘要与要点取自仓库 `README.md`（经 API 的 base64 解码）。
- `published` 填仓库创建日期 `created_at`（2026-06-09），仓库无正式发布日期；`source` 为 Plannotator。
- star 数为抓取当日快照（3,683 stars，2026-10-18），会随时间变化；仓库 MIT 许可，主语言 HTML。
