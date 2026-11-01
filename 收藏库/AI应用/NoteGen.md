---
title: "NoteGen"
url: "https://github.com/codexu/note-gen"
source: "GitHub / codexu"
published: "2024-08-06"
archived: "2026-10-02"
category: "AI应用"
tags: [Markdown, 知识管理, RAG, Tauri, MCP]
lang: en
status: ok
---

# NoteGen

> 一句话定位：以"先记录、后整理"为理念的跨平台 Markdown 笔记应用，用 AI 把截图、链接、片段等碎片化输入逐步整理成可读笔记。

## 摘要

由 codexu 开发，基于 Tauri2，覆盖 Windows、macOS、Linux（beta）与 Android、iOS（alpha），安装包约 20MB，支持离线使用与历史回滚。工作区分三个空间：Recording 捕获想法、截图、链接与临时输入；Notes 用 Markdown 编辑器整理内容，支持表格、图表与数学公式；AI Dialogue 与笔记和知识库对话，复用上下文完成整理与重写。存储坚持原生 Markdown 以保证便携性，内置知识库支持 RAG、向量索引与混合检索（含嵌入与重排序模型），可通过 MCP、自定义模型、提示词、记忆与 Agent 工作流扩展；同步支持 GitHub、Gitee、GitLab、Gitea、S3、WebDAV。截至 2026-10-02 约 12.9k stars、974 forks，GPL-3.0。

## 要点

- 三空间工作流（Recording / Notes / AI Dialogue）把碎片捕获与结构化整理分离，避免记录阶段被格式问题打断。
- 存储策略是原生 Markdown 而非私有格式，换取可移植性与长期兼容性；同步后端可选 GitHub、Gitee、GitLab、Gitea、S3、WebDAV。
- 内置知识库支持 RAG、向量索引与混合检索，同时使用嵌入模型与重排序模型提升召回质量。
- 扩展面较宽：MCP、自定义模型、提示词、记忆与 Agent 工作流均可接入，模型侧兼容 ChatGPT、Gemini、Ollama、LM Studio、Grok。
- 关注度：约 12.9k stars、974 forks；GPL-3.0 许可，主语言 TypeScript；创建于 2024-08-06，最后推送 2026-09-30；topics 含 local-first、rag、mcp、tauri、webdav。

## 关联

- 与 `其他/Zettlr.md` 同为 Markdown 笔记/写作工具，可对比本地优先与 AI 集成的取舍。
- 与 `AI应用/Prompts.chat.md` 相关：两者都通过 MCP 把外部能力接入日常工具链。
- 与 `Agent框架/Mem0.md` 在记忆与上下文复用主题上相关。

## 注意

- 源笔记称许可证为"开源"未指明具体协议，本条目按 API 返回的 GPL-3.0 记录。
- 平台成熟度（beta / alpha、iOS 走 TestFlight）来自源笔记，未实测；星标/forks 为 2026-10-02 快照。
- 仓库创建于 2024-08-06，`published` 取该创建日期；仓库无单独发布日期。
