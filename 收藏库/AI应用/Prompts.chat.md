---
title: "Prompts.chat"
url: "https://github.com/f/prompts.chat"
source: "GitHub / f"
published: "2022-12-05"
archived: "2026-10-02"
category: "AI应用"
tags: [提示工程, MCP, 自托管, 提示词库, 开源]
lang: en
status: ok
---

# Prompts.chat

> 一句话定位：原名 Awesome ChatGPT Prompts 的开源提示词库，既提供可直接取用的社区提示词，也支持企业自托管成私有提示词库。

## 摘要

由开发者 f 维护，自称最早的提示词库（2022 年 12 月创建），支持 ChatGPT、Claude、Gemini、Llama 等模型，提供 prompts.chat、prompts.csv、PROMPTS.md 与 Hugging Face Dataset 分发格式并开放社区投稿；另含 25+ 章免费交互式提示工程书籍（覆盖链式思维、少样本学习与 AI 代理）和面向 8–14 岁儿童的游戏化学习内容。工程重点是自托管与集成：`npx prompts.chat new` 可起一个带自定义品牌与 GitHub / Google / Azure AD 认证的私有库，并可经 CLI、Claude Code 插件或 MCP 服务器接入。源代码 MIT，内容与数据 CC0 1.0。截至 2026-10-02 约 171.9k stars、22k forks。

## 要点

- 双重许可区分代码与内容：源代码 MIT，提示词内容与数据 CC0 1.0 公共领域，便于企业内部二次利用。
- 三种接入方式：`npx prompts.chat` CLI、Claude Code 插件市场，以及 MCP 服务器（远程 URL 或本地 npx 启动），可把提示词库当作工具接入 AI 客户端。
- 自托管面向隐私诉求，支持自定义品牌、主题与 GitHub / Google / Azure AD 认证，官方提供 Docker 与自托管文档。
- 教育侧资源分层：25+ 章交互式提示工程书面向开发者，游戏化冒险面向 8–14 岁儿童。
- 关注度与认可：约 171.9k stars、22k forks；创建于 2022-12-05，最后推送 2026-10-01；源笔记称其被 Forbes 报道、被高校引用并有 40+ 学术引用，为 Hugging Face 热门数据集与 GitHub Staff Pick。

## 关联

- 与 `AI应用/NoteGen.md` 相关：两者都支持 MCP 接入并把 AI 能力嵌入日常工具。
- 与 `SKILL/Anthropic-Skills.md`、`AI编程/Claude-Context.md` 相关，同属"提示/技能资产"体系，可通过 Claude Code 插件与 MCP 组合使用。

## 注意

- 源笔记标注 stars 为 "143k+"，与 2026-10-02 API 返回的 171,854 有差异，本条目采用 API 值。
- API 返回许可证为 NOASSERTION，源笔记说明为"源码 MIT + 内容 CC0 1.0"的双重许可，未在 API 中体现，采用源笔记描述。
- 源笔记所称 Forbes 报道、40+ 学术引用、Hugging Face 数据集排名等认可信息来自项目自述，本次未独立核验。
- 仓库创建于 2022-12-05，`published` 取该创建日期；仓库无单独发布日期。
