---
title: "Frontend Slides"
url: "https://github.com/zarazhangrui/frontend-slides"
source: "GitHub / zarazhangrui"
published: "2026-01-28"
archived: "2026-10-02"
category: "SKILL"
tags: [Claude Code, Agent Skill, HTML 演示文稿, PPT 转换, 渐进式披露]
lang: en
status: ok
---

# Frontend Slides

> 一句话定位：一个给编码智能体（coding agent）用的技能包，让不懂 CSS/JS 的人也能做出好看的 HTML 网页演示文稿，还能把现成的 PPT 转成网页版。

## 摘要

Frontend Slides 是一个面向编码智能体的开源技能（skill），打包为 Claude Code 插件，其核心 `SKILL.md` 也能被 Codex、Gemini CLI 等具备文件系统与 shell 权限的智能体读取。它解决的是"非设计师做不出好看的幻灯片"这一问题：产物是**零依赖的单文件 HTML**（CSS/JS 全部内联），没有 npm、构建工具和框架。它的设计思路是"展示而非描述"——不要求用户用语言描述审美偏好，而是先根据需求生成 3 个视觉风格预览让用户挑，再据此生成整套 16:9 的演示文稿。此外支持把现有 PowerPoint 文件（文本、图片、备注）转换成网页幻灯片，并内置 12 套精选风格与可选的 34 套 bold 模板包，风格上刻意避开常见的"紫色渐变 + 白底"式 AI 味。工程思路上采用渐进式披露：主 `SKILL.md` 只是工作流地图，其余参考文件按需加载。

## 要点

- **零依赖交付物**：单 HTML 文件内联 CSS/JS，作者观点是"依赖就是债务"——单文件十年后仍能打开，2019 年的 React 项目则未必。
- **风格发现流程**：先出 3 个预览（1 个安全预设 + 至少 1 个 bold 模板 + 1 个野牌/自定义），用户选定后才生成完整稿并自动在浏览器打开。
- **PPT 转换**：用 `scripts/extract-pptx.py` 提取 pptx 的文本、图片与备注，确认内容后选风格再生成，保留原始素材。
- **渐进式披露的目录设计**：`STYLE_PRESETS.md`、`html-template.md`、`animation-patterns.md`、`viewport-base.css` 等按阶段加载；bold 模板先只读紧凑索引，入选后才读 `preview.md`，最终只对选中的那一套读完整 `design.md`。
- **分享与导出**：`scripts/deploy.sh` 一键部署到 Vercel 拿到可分享 URL；`scripts/export-pdf.sh` 用 Playwright 以 1920×1080 逐页截图合成 PDF（不保留动画）。
- **关注度**：截至 2026-10-02 抓取时，仓库 30,063 stars、2,351 forks，MIT 许可，主语言 JavaScript；仓库创建于 2026-01-28，最后一次推送 2026-06-23。

## 关联

- 同属面向编码智能体的可复用能力包，可与 `SKILL/Anthropic-Skills.md`（官方 Skills 集合）、`SKILL/Superpowers.md`（技能化软件开发方法论）、`SKILL/Agency-Agents.md`（代理角色集合）、`SKILL/ArcKit.md`（企业架构命令包）对照阅读。
- 与 `AI编程/Everything-OpenCode.md` 的 Skills/命令扩展体系属于同一类扩展机制的不同宿主。

## 注意

- GitHub 仓库页面 `web_fetch` 只返回导航外壳与截断内容，摘要与要点取自仓库 `README.md` 原文（raw 地址）；星标/forks/日期等元数据来自 GitHub API，为抓取当日快照，会随时间变化。
- `published` 填的是仓库创建日期（2026-01-28），仓库本身无正式发布日期；`source` 为 GitHub 账号 zarazhangrui。
- 仓库中引用的 YouTube 讲解视频与模板画廊截图未逐一核验。
- 2026-10-02 分类由原 `技术/AI` 迁移为 `SKILL`（原路径 `技术/AI/skills/Frontend-Slides.md`），以统一全库一级分类。
