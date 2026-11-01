---
title: "Follow Builders"
url: "https://github.com/zarazhangrui/follow-builders"
source: "GitHub / zarazhangrui"
published: "2026-03-14"
archived: "2026-10-07"
category: "SKILL"
tags: [Claude Code, OpenClaw, Agent Skills, AI 资讯, 内容聚合]
lang: en
status: ok
---

# Follow Builders

> 一句话定位：一个 Agent Skill（"Follow Builders, Not Influencers"），让智能体每天/每周把 6 档 AI 播客、26 位 AI builder 的 X 帖子与两家官方博客 remix 成带链接的摘要，推送到你常用的消息渠道。

## 摘要

Follow Builders 是 Zara Zhang（@zarazhangrui）开源的 Agent Skill，把 AI 行业动态做成一档可对话配置的定期摘要：每天或每周汇总 6 档 AI 播客的新节目、26 位 AI builder 的 X 帖子，以及 Anthropic Engineering 与 Claude Blog 的官方文章，输出带原文链接的英文、中文或双语摘要，投递到 Telegram、邮件或直接在对话中显示。设计取向是"用户零配置"——原始内容由中央 feed 每日更新，用户端只发一次 HTTP 请求、无需任何 API key，摘要风格由 `prompts/` 下的纯英文提示词控制并可对话修改。运行职责拆得干净：`prepare-digest.js` 确定性地取回配置、内容与提示词并输出单个 JSON，智能体只按提示词 remix，并被明确禁止联网或编造内容。MIT 许可，仓库创建于 2026-03-14。

## 要点

- **三源合一的 AI 动态摘要**：播客 6 档（Latent Space、Training Data、No Priors、Unsupervised Learning、The MAD Podcast、AI & I by Every）、X 上 26 位 builder（Karpathy、swyx、Sam Altman、Guillermo Rauch、Amjad Masad、Aaron Levie、Zara Zhang 等）、官方博客 2 个（Anthropic Engineering、Claude Blog）。
- **零 API key 架构**：内容由中央 feed 每日更新（博客走网页抓取、YouTube 转录走 Supadata、X 走官方 API），用户端 `prepare-digest.js` 一次请求取回全部 JSON；只有选择 Telegram / Email 投递时才需要本地 `~/.follow-builders/.env` 中的密钥。
- **提示词即配置文件**：`prompts/` 下 5 个纯英文文件（summarize-podcast、summarize-tweets、summarize-blogs、digest-intro、translate）决定摘要风格；用户改动会复制到 `~/.follow-builders/prompts/`，从而不被中央更新覆盖。
- **防幻觉约束写进流程**：脚本负责取数，智能体只能 remix JSON 中已有内容，禁止访问 x.com 或自行搜索；每条内容必须带 URL，职位不得猜测（只能用 `bio` 字段）。
- **平台差异决定交付方式**：OpenClaw 用内置消息渠道 + `openclaw cron add`（须显式指定 channel 与 target，文档明确警告不要用 `--channel last`）；Claude Code / Cursor 等非持久化智能体需要 Telegram 或 Email + 系统 crontab，否则只能手动 `/ai` 触发——且 crontab 直投路径不经过 LLM remix，只会发出原始 JSON。
- **语言与频率可配**：每日或每周定时推送，语言支持英文 / 中文 / 双语；双语要求按人按段中英交替，而不是先全英再全中。

## 关联

- `SKILL/Frontend-Slides.md` — 同一作者 zarazhangrui 的另一个 Agent Skill，可作为其技能打包与"渐进式披露"设计手法的对照。
- `SKILL/Anthropic-Skills.md` — 本条目遵循的 `SKILL.md` + frontmatter 规范与渐进式披露模式的官方参考。
- `其他/NewsNow.md` — 同为"多源信息聚合"思路；差别在于 NewsNow 面向通用热点新闻并需自托管，本条目面向 AI 圈人物动态且由中央 feed 托管。

## 注意

- `github.com` 页面 `web_fetch` 与自带抓取器均连接超时，正文取自 `raw.githubusercontent.com` 的 `README.md`、`SKILL.md`、`config/default-sources.json` 与 `examples/sample-digest.md`；仓库元数据取自 GitHub API。
- README 声明 MIT 许可，但 GitHub API 的 `license` 字段为空，未能交叉核验。
- 仓库无正式发布日期，`published` 填的是创建日期 2026-03-14；stars 6,844 / forks 892 / 最后推送 2026-10-07 为 2026-10-07 的 API 快照，会随时间变化。
- 仓库未设置 topics；`examples/sample-digest.md` 中的播客与推文链接为 `example123` 一类占位示例，非真实内容。
