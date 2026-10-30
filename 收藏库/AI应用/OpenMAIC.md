---
title: "OpenMAIC"
url: "https://github.com/THU-MAIC/OpenMAIC"
source: "GitHub / THU-MAIC"
published: "2026-03-11"
archived: "2026-10-02"
category: "AI应用"
tags: [多智能体, AI教育, 课程生成, 幻灯片, OpenClaw]
lang: en
status: ok
---

# OpenMAIC

> 一句话定位：清华大学团队开源的多智能体互动课堂，把任意主题或文档一键转成由 AI 教师与 AI 同学讲授、含幻灯片与测验的完整课程。

## 摘要

OpenMAIC（Open Multi-Agent Interactive Classroom）由清华大学 THU-MAIC 维护。它接收主题或上传材料后先由 AI 生成课程大纲，再把每个大纲项展开为幻灯片、测验、互动 HTML 模拟或项目式学习（PBL）活动；课堂由 AI 教师与 AI 同伴实时授课讨论，支持语音讲解、白板绘图与公式书写，并可导出 .pptx 或互动 .html。模型兼容 OpenAI、Anthropic、Gemini、DeepSeek、MiniMax、Grok 及任意 OpenAI 兼容 API，推荐 Gemini 3 Flash。部署支持本地 pnpm、Vercel、Docker，并可经 OpenClaw 从 20+ 消息应用生成课堂。截至 2026-10-02 约 39.8k stars、6.17k forks，MIT 许可。

## 要点

- 两阶段生成流水线：大纲阶段产出结构化课程结构，场景阶段把每个大纲项物化为幻灯片 / 测验 / 互动模拟 / PBL，便于控制生成质量。
- 多智能体课堂不止生成内容：AI 教师用语音、聚光灯与激光指针动画授课，AI 同学参与实时讨论，白板支持绘图与公式。
- 模型无关：支持 OpenAI、Claude、Gemini、DeepSeek、MiniMax、Grok 及任意 OpenAI 兼容端点，推荐 Gemini 3 Flash。
- 交付灵活：导出 .pptx 与 .html；部署可选本地 pnpm、Vercel 或 Docker Compose；另有托管模式（open.maic.chat 访问码）免本地配置。
- 关注度：约 39.8k stars、6.17k forks；MIT 许可，主语言 TypeScript，创建于 2026-03-11，最后推送 2026-10-02。

## 关联

- 与 `教程/Awesome-OpenClaw-Tutorial.md`、`教程/Awesome-OpenClaw-Usecases.md` 相关：OpenMAIC 通过 OpenClaw 技能从 20+ 消息应用生成课堂。
- 与 `SKILL/Frontend-Slides.md` 在幻灯片生成主题上互补：后者产出单文件 HTML 演示稿，前者产出含讲授与测验的完整课堂。
- 与 `生成工具/MarkItDown.md` 相关：都涉及把外部材料（文档 / PDF）转成结构化可用内容。

## 注意

- GitHub API 未返回该仓库的 topics（为空），故本条目标签依据源笔记与仓库描述归纳。
- 源笔记标注版本 v0.1.0（2026-03-26 发布），本次 API 抓取未核验 release 列表；`published` 取仓库创建日期 2026-03-11。
- 星标/forks 为 2026-10-02 快照；托管模式访问码与 MinerU 集成细节未实测。
