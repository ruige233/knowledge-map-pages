---
title: "OpenDesign：首个协作式 AI 设计 Agent 工作台"
url: "https://github.com/nexu-io/open-design/blob/main/docs/i18n/README.zh-CN.md"
source: "GitHub / nexu-io"
published: ""
archived: "2026-10-29"
category: "设计工具"
tags: [OpenDesign, AI设计, Agent, MCP, 设计系统]
lang: zh
status: ok
---

# OpenDesign：首个协作式 AI 设计 Agent 工作台

> 一句话定位：从需求描述出发、调用你已有的编码 Agent 按品牌规范渲染原型/Deck/图片/视频的开源协作式 AI 设计工作台。

## 摘要

OpenDesign 是 nexu-io 开源的协作式 AI 设计 Agent 工作台（Apache-2.0），自称"首个"此类产品：用户从需求描述出发，让 OpenDesign 调用 PATH 上已有的编码 Agent（Claude Code、Codex、Cursor、Copilot 等 26 个 CLI）或自带 OpenDesign Cloud，按 `DESIGN.md` 品牌契约渲染符合设计系统的原型、Deck、图片与视频。它不发布自己的 Agent、模型无关、本地优先，通过技能、CLI 与 stdio MCP 三种方式接入 Agent，并用插件、功能技能、设计模板、设计系统四个可组合层组织能力。仓库随附 100+ 功能技能、151 个设计系统包、277 个官方插件，工件可导出为 HTML/PDF/PPTX/MP4 等，BYOK 代理在边缘提供 SSRF 防护。

## 要点

- **定位与机制**：不绑定模型、不发布 Agent，直接把用户已有的编码 Agent 当作"设计引擎"，一键切换；每次渲染读取当前包的 `DESIGN.md` 作为核心品牌契约。
- **接入方式**：`od mcp install <agent>` 一行安装到 16+ CLI（claude、codex、cursor、copilot、opencode、openclaw、hermes、kimi、cline、trae 等）；DeepSeek Harness 为一等原生运行时（`od agent setup deepseek-harness`）。
- **四个可组合层**：插件承载工作流、功能技能承载 Agent 行为、设计模板承载渲染蓝图、设计系统承载品牌（151 个包，以 `DESIGN.md` 为核心）。
- **产物与导出**：原型、实时仪表盘/决策室、Deck、图片、HyperFrames 视频；Deck 可导出 HTML/PDF/PPTX/ZIP/Markdown，视频经 headless Chrome + FFmpeg 渲染为确定性 MP4。
- **架构**：Next.js 16 + React 前端、Electron 外壳、Node 24 + Express + better-sqlite3 守护进程；base registry 含 27 个 runtime 定义、26 个独立 CLI 可执行程序；BYOK 代理 `POST /api/proxy/{anthropic,openai,azure,google,ollama,senseaudio}/stream` 在边缘拦截 SSRF。
- **插件系统**：277 个官方插件 + 183 个参考示例，`open-design.json`（specVersion 1.0.0）为核心 manifest，`od plugin scaffold/validate/apply` 全流程支持，可按最小权限声明 capabilities。

## 关联

- `设计工具/Awesome-Design-MD.md`：OpenDesign 的 `DESIGN.md` schema 与 70 个上游衍生设计系统的历史来源之一正是 VoltAgent/awesome-design-md。

## 注意

- GitHub API 因限流（403 rate limit exceeded）无法取得仓库创建/首次提交日期与 star 数等元数据，`published` 留空。
- README 顶部宣传横幅称"庆祝 GitHub 100K Star"，属滚动营销文案，未作为固定数据写入要点。
