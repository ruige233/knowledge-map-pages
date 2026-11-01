---
title: "Awesome DESIGN.md"
url: "https://github.com/VoltAgent/awesome-design-md"
source: "GitHub / VoltAgent"
published: "2026-03-31"
archived: "2026-10-02"
category: "设计工具"
tags: [DESIGN.md, 设计系统, Google Stitch, 设计令牌, 编码智能体]
lang: en
status: ok
---

# Awesome DESIGN.md

> 一句话定位：把各大知名品牌的视觉规范整理成一份份纯 Markdown 的 DESIGN.md，丢进项目里就能让 AI 编码智能体照着产出风格一致的 UI。

## 摘要

DESIGN.md 是 Google Stitch 引入的一种约定：用一个纯文本 Markdown 文件描述项目的视觉规范，AI 智能体读它来生成界面。它能成立的关键在于 Markdown 是 LLM 阅读效果最好的格式——不需要 Figma 导出、JSON schema 或任何专有工具，放进项目根目录就能被编码智能体或 Stitch 直接理解。这个仓库（VoltAgent 维护）做的就是把众多流行品牌的设计语言逐一提炼成这样的文件：不是截图，而是可读的规范描述，涵盖配色、字体、留白、圆角、动效倾向等。覆盖面按领域分组，包含 AI & LLM 平台、开发者工具与 IDE、后端/数据库/DevOps、生产力 SaaS、设计与创意工具、金融科技与加密、电商零售等类目，条目数在 50 以上并接受社区追加。和 AGENTS.md 的分工很清晰：后者告诉智能体项目怎么搭，DESIGN.md 告诉它项目该长什么样。热度上这是一个罕见的高关注度资源库，2026-10-02 快照约 119.2k stars、13.3k forks，MIT 许可。

## 要点

- **一个文件替代一套设计交付物**：DESIGN.md 是纯 Markdown，无需 Figma 导出、JSON schema 或专用工具，任何编码智能体与 Google Stitch 都能直接读。
- **与 AGENTS.md 的分工**：AGENTS.md 定义"如何构建项目"（给编码智能体），DESIGN.md 定义"外观与感觉"（给设计智能体）。
- **使用步骤极短**：从仓库复制一份 DESIGN.md 到项目 → 对智能体说"build me a page that looks like this" → 得到风格匹配的 UI。
- **覆盖领域**：AI/LLM 平台（Claude、Mistral、Ollama、xAI 等）、开发者工具与 IDE（Cursor、Vercel、Raycast 等）、后端与 DevOps（ClickHouse、Supabase、Sentry 等）、生产力 SaaS（Linear、Notion 等）、设计创意（Figma、Framer 等）、金融科技（Stripe、Coinbase 等）、电商零售（Airbnb、Nike 等）。
- **关注度数据**：2026-10-02 快照约 119.2k stars / 13.3k forks，MIT 许可，仓库创建于 2026-03-31。

## 关联

- 与 `技术/AI/skills/Frontend-Slides.md` 同属"让智能体产出好看前端"的路线：DESIGN.md 提供视觉规范，Frontend Slides 提供演示文稿生成工作流，可搭配使用。
- 与 生成工具/MarkItDown.md 无直接关系，仅在"LLM 友好格式"这一理念上相通。

## 注意

- 来源笔记整理了各品牌的风格描述表格，本条只保留领域概览与代表性品牌，未逐条搬运。
- 仓库主语言字段在 GitHub API 中为 null（内容以 Markdown 为主），故未在摘要中断言语言。
- 来源笔记引用的官方网站 getdesign.md 与 Google Stitch 页面未逐一核验。
