---
title: "DeepWiki-Open"
url: "https://github.com/AsyncFuncAI/deepwiki-open"
source: "GitHub / AsyncFuncAI"
published: "2025-04-30"
archived: "2026-10-02"
category: "AI编程"
tags: [RAG, Mermaid, Next.js, 文档生成, 代码分析]
lang: en
status: ok
---

# DeepWiki-Open

> 一句话定位：DeepWiki 的开源复现，输入仓库地址就自动生成带架构图的交互式 Wiki，并支持基于 RAG 的仓库问答。

## 摘要

DeepWiki 的开源实现：输入 GitHub、GitLab 或 BitBucket 仓库地址，AI 自动分析代码结构、生成交互式 Wiki 文档、用 Mermaid 绘制架构与数据流和模块关系图，并产出便于按代码路径浏览的 codemap。它还提供基于 RAG 的对话能力，可直接就仓库提问，以及多轮研究的 DeepResearch 流程。模型侧支持 Google Gemini、OpenAI、OpenRouter 与本地 Ollama，嵌入模型可选 OpenAI、Google AI 或 Ollama 本地模型；私有仓库通过个人访问令牌访问。技术栈为 Next.js + TypeScript + Tailwind CSS 前端与 Python FastAPI 后端，docker-compose 一键部署，MIT 许可。README 显示 2.0 版本（Grok Wiki）已上线并提供了下载站点。

## 要点

- **产出物**：结构化 Wiki 页面 + Mermaid 架构/数据流/模块关系图 + codemap 代码导览，适用于代码审查、新人 onboarding 与技术调研。
- **模型与嵌入可插拔**：对话支持 Gemini、OpenAI、OpenRouter、Ollama；嵌入支持 OpenAI Embeddings、Google AI Embeddings 与本地 Ollama，可按成本与性能取舍。
- **部署方式**：`docker-compose up` 一键启动，本地开发则为前端 `npm run dev` 加 `api/` 目录下 Python 服务。
- **使用体验**：源笔记称多语言 README 覆盖 9 种语言（含中、英、日、韩等），支持 LaTeX 公式渲染与暗色模式。
- **项目阶段**：README 标注 2.0（Grok Wiki）已上线并给出下载地址，README 主体因此较短，社区数据（216+ 提交、198+ Issues、14 个分支）为源笔记 2026-05-16 时点。
- **关注度**：截至 2026-10-02 快照，仓库 18,112 stars、2,014 forks，创建于 2025-04-30，最后一次推送 2026-09-03。

## 关联

- `AI编程/Claude-Context.md` — 同批条目，都让模型理解陌生代码库：本条生成文档与问答入口，那条提供编码时的语义检索。
- `AI编程/Context-Mode.md` — 同批条目，同属控制进入模型上下文的代码/工具内容量。

## 注意

- GitHub API 两次调用均返回 403（`API rate limit exceeded`，共享出口 IP 触发未认证限额），因此改用 GitHub 数据代理 `ungh.cc` 补齐元数据：`published` 为仓库创建日期 2025-04-30，stars/forks 为 2026-10-02 快照，会随时间变化；未取到许可证字段，MIT 许可由仓库 README 确认。
- README 兜底抓取成功（HTTP 200），但内容简短，除 MIT 许可与该入口外未提供更多元数据。
- 源笔记中的 "216+ 提交 / 198+ Issues / 14 分支" 为 2026-05-16 时点数据，本条目未复核。
