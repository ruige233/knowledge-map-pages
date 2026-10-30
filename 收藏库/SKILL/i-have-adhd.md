---
title: "i-have-adhd"
url: "https://github.com/ayghri/i-have-adhd"
source: "ayghri (GitHub)"
published: "2026-05-13"
archived: "2026-10-18"
category: "SKILL"
tags: [Claude Code, ADHD, 编码智能体, 提示词, 工作流]
lang: en
status: ok
---

# i-have-adhd

> 一句话定位：一个让编程助手输出"行动优先、对 ADHD 友好"的技能包，把答案直接放到最前，而不是埋进寒暄和铺垫。

## 摘要

i-have-adhd 是一个面向编程助手（Claude Code 等）的技能/插件，目标是让 AI 的回答"对 ADHD 友好"：行动优先、结论前置，不绕弯子。它定义 10 条规则——先给下一步行动、多步任务用编号、结尾给一个具体下一步、抑制跑题、每轮重述当前状态、用分钟而非"一会儿"估算时间、让进展可见、平铺直叙地报错、列表不超过 5 条、去掉开场白/回顾/收尾。安装只需在 CLI 提示词里粘贴一句安装指令，或按 INSTALL.md；也可 fork 后替换 SKILL.md 自定义。README 用 Before/After 对照展示同一个 auth 问题，回答从冗长客套（"Great question… Hope this helps!"）变成直接可执行（"Edit src/auth.ts:42… Next: …"）。该技能灵感来自《The Adult ADHD Tool Kit》，把书中针对 ADHD 成年人的组织建议改写成 LLM 应当如何回应，而非人类如何安排一天。MIT 许可。

## 要点

- 核心主张：让编码助手把结论与下一步放在最前，避免"先寒暄再铺垫"式回答。
- 10 条规则：先行动、编号、单一下一步、压跑题、重述状态、分钟级估算、可见进展、平实报错、列表≤5、无开场白/回顾/收尾（全文见 skills/i-have-adhd/SKILL.md）。
- 安装：在 CLI 提示词粘贴安装指令，或参考 INSTALL.md；可 fork 后替换 SKILL.md 再 `claude plugin install` 自定义版本。
- Before/After 对照：同一 auth 问题从多段客套缩为"改 `src/auth.ts:42` → 跑测试 → 给出失败行"。
- 灵感来源：《The Adult ADHD Tool Kit》（J. Russell Ramsay、Anthony L. Rostain），改写为 LLM 的回应方式。
- MIT 许可；star 数约 5.8 万（2026-10-11 抓取快照，滚动值，会随时间变化）。

## 关联

- 与本库 `SKILL/` 下的其他编码助手技能包（如 `Anthropic-Skills`、`Superpowers`）同类，都是可挂载进 Claude Code 的能力包。

## 注意

- `published` 取仓库创建日期 `created_at`（2026-05-13）；star 数为 GitHub 快照值，非固定数据。
- 仓库 `language` 字段为 Python（仅为主语言统计），实际内容以 SKILL.md（Markdown）为主。
