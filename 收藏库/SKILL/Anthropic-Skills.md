---
title: "Anthropic Skills"
url: "https://github.com/anthropics/skills"
source: "GitHub / anthropics"
published: "2025-09-22"
archived: "2026-10-18"
category: "SKILL"
tags: [Claude Skills, Agent Skills, SKILL.md, MCP, Anthropic]
lang: en
status: ok
---

# Anthropic Skills

> 一句话定位：Anthropic 官方的 Agent Skills 仓库，既收录可直接使用的 Skill 示例，也是编写自定义 Skill 的规范与最佳参考。

## 摘要

Anthropic Skills 是 Anthropic 官方维护的 Agent Skills 仓库，既收录可直接使用的 Skill 示例，也是编写 Skill 的规范参考。每个 Skill 是一个自包含文件夹，核心为带 YAML frontmatter（必需字段 name、description）的 `SKILL.md`，Claude 按用户请求动态加载以提升特定任务表现。当前 `skills/` 目录共 19 个 Skill，主体分四类：创意设计（algorithmic-art、canvas-design、frontend-design、theme-factory、slack-gif-creator、web-artifacts-builder）、开发技术（claude-api、mcp-builder、skill-creator、webapp-testing）、企业通讯（brand-guidelines、internal-comms、doc-coauthoring）与文档处理（docx、pdf、pptx、xlsx），另新增 academy-guide（推荐 Claude Academy 课程）与 discernment-nudge（作答前自查的元技能）。文档类 Skill 为 source-available 而非开源。仓库除 `skills/` 外还含 `spec/`（Agent Skills 规范，官网 agentskills.io）与 `template/`（Skill 模板），可通过 Claude Code 插件市场、Claude.ai 或 Skills API 使用，并开始收录 Partner Skills（如 Notion）。仓库创建于 2025-09-22，API 快照 181,926 stars、21,549 forks。

## 要点

- **Skill 的结构**：一个文件夹加一个 `SKILL.md`，frontmatter 里 `name`（小写、空格转连字符）与 `description` 为必需字段；Claude 按请求动态加载，而不是一次性塞进上下文。
- **共 19 个 Skill**：四类主体 17 个（创意设计 6、开发技术 4、企业通讯 3、文档处理 4）+ 新增 academy-guide、discernment-nudge 两个元技能。
- **元技能 skill-creator**：覆盖从需求捕获、SKILL.md 撰写、评估运行到 description 优化的完整生命周期，是上手自定义 Skill 的入口。
- **规范与模板**：`spec/` 定义 Agent Skills 规范（官网 agentskills.io）；`template/` 提供最小可用的 Skill 起始模板。
- **使用方式**：Claude Code 用 `/plugin marketplace add anthropics/skills` 注册插件市场后安装 document-skills 或 example-skills；Claude.ai 对付费用户开放示例 Skill，也可通过 Claude API 的 Skills API 上传或使用。
- **许可证分层**：仓库内多数 Skill 为 Apache-2.0，文档类 Skill（docx/pdf/pptx/xlsx）为 source-available。

## 关联

- `SKILL/Superpowers.md` — 同属编码代理技能生态：本仓库提供官方规范与示例，Superpowers 提供方法论驱动的技能集合。
- `SKILL/Frontend-Slides.md` — 第三方 Skill 按 SKILL.md 规范与渐进式披露思路实现，可作为对照实例。

## 注意

- GitHub API 未返回 license 字段（`license.spdx_id` 为空），许可证信息取自 README：多数 Skill 为 Apache-2.0，文档类 Skill 为 source-available。
- Stars/Fork 为滚动值：本次 API 快照（2026-10-18）为 181,926 / 21,549，与 2026-10-02 快照 179,384 / 21,211 的差距属时间不同，勿当作精确常量。
- 旧条目记「17 个 Skill」；经 `contents/skills` 目录核实现为 19 个（新增 academy-guide、discernment-nudge）。
