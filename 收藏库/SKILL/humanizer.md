---
title: "Humanizer"
url: "https://github.com/blader/humanizer"
source: "GitHub · blader"
published: "2026-01-18"
archived: "2026-11-03"
category: "SKILL"
tags: [Humanizer, AI写作, Agent Skills, Claude Code, Codex]
lang: en
status: ok
---

# Humanizer

> 一句话定位：一个 Agent 技能包，按维基百科"AI 写作痕迹"指南重写 AI 文本，让文字像人写的，同时不改变原意。

## 摘要

Humanizer 是一个开源 Agent 技能（Skill），可安装到 Claude Code、Codex 及任何支持 Agent Skills 的智能体，以 `/humanizer` 命令调用。它基于维基百科《Signs of AI writing》整理出 26 种 AI 写作痕迹模式（Not X but Y、单句收尾、假装深刻、铺垫式开场、与空气辩论、强制三连、装饰性加粗、销售腔、借用权威等），按强度与频率编号，重写时逐条标记并消除这些痕迹。其目标不是骗过 AI 检测器，而是让文字更适合人类阅读；重写保留原文事实，不编造姓名、数字、日期、引文等细节，并可按用户提供的写作样本匹配个人文风。仓库 MIT 协议，约 6.4 万 star。

## 要点

- **定位**：去除 AI 写作痕迹的 Agent 技能，不改变原意；目标读者是人，而非通过 AI 检测器（检测器仍会标记大部分输出）。
- **依据**：维基百科《Signs of AI writing》，共 26 种模式，分 6 组——铺垫代替陈述、按规则制造节奏、拔高与借用权威、按规则排版、聊天/草稿残留、面向错误读者。
- **五种最强信号**：Not X but Y、单句收尾、听起来深刻的话、铺垫式开场、与空气辩论——单独出现即可触发改写。
- **安装**：Claude Code 用 `/plugin marketplace add blader/humanizer` + `/plugin install humanizer@humanizer`（需 2.1.142+，旧版用 `npx skills add`）；Codex 及其他智能体用 `npx skills add blader/humanizer --global --agent <agent>`；Claude.ai / Desktop 上传 ZIP 即可。
- **用法**：`/humanizer` 后粘贴文本，或直接说 "please humanize this text"；可指定文件路径批量改写 prose，也可先给 2–3 段本人文字做文风匹配。
- **质量约束**：改写保留事实细节，不改代码、数据、frontmatter、链接；缺少必要细节时提问而非编造。盲测 16 次中 16 次评审更偏好 Humanizer 的改写（issue #229）。

## 注意

- README 中的 star 数为滚动值：本次抓取为 64,247（2026-11-03），会随时间变化。
- `published` 取仓库创建日期（2026-01-18）；最近一次推送为 2026-10-28。
