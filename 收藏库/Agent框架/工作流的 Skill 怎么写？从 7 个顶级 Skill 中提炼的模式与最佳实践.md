---
title: "工作流的 Skill 怎么写？从 7 个顶级 Skill 中提炼的模式与最佳实践"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247559690&idx=1&sn=fed34d2ade198ea8fb75ad5b768c9229"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent框架, Skill]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/工作流的 Skill 怎么写？从 7 个顶级 Skill 中提炼的模式与最佳实践.html"
---

# 工作流的 Skill 怎么写？从 7 个顶级 Skill 中提炼的模式与最佳实践

> 一句话定位：从 7 个顶级 Skill 提炼 5+1 种设计模式与 frontmatter、写作最佳实践

## 摘要

文章把 Skill 定义为以 SKILL.md 为核心的知识注入机制：SKILL.md 全文被注入上下文，LLM 用既有工具执行指令，而非生成新工具。frontmatter 的 name 与 description 决定加载率，description 应包含触发短语、时序位置与产品关键词。作者从 7 个顶级 Skill 提炼出 5 种核心模式——线性流程、决策树+按需加载、循环迭代、接力棒循环、多阶段+检查点，外加思维框架这一特殊模式，并给出每种模式的适用场景、代表仓库与关键技巧。随后归纳通用写作技巧：防偷懒四武器、三种教学方式、安全边界三原则、三层知识组织架构及 token 预算，最后给出模式选择决策树、两类快速上手模板、参考资源与 7 个 Skill 速查表。

## 要点

- Skill 是文件夹+SKILL.md，本质是知识注入，不动态生成新工具
- frontmatter 必填 name 与 description；description 决定 LLM 是否加载
- 好的 description 包含触发短语、时序位置和产品关键词
- 5 种核心模式：线性流程、决策树+按需加载、循环迭代、接力棒循环、多阶段+检查点
- 特殊模式思维框架控制 LLM“怎么想”，用于安全审计等深度分析
- 防偷懒四武器：强硬语气、借口反驳表、量化阈值、负面指令
- 知识组织三层架构：frontmatter 约 100 tokens、主文件 2K-5K、references 按需加载
- 用决策树按操作类型选择模式，另附线性与循环迭代两套上手模板

## 提炼

> 一句话定位：从 7 个顶级 Skill 提炼 5+1 种设计模式与 frontmatter、写作最佳实践

### 分节提炼

- **01 Skill 本质**：Skill 是文件夹，核心是 SKILL.md（YAML frontmatter + Markdown 正文）；scripts/references/resources/examples 为可选目录；本质是知识注入：指令文本进上下文，LLM 用 bash/read/edit 等已有工具执行
- **02 Frontmatter 门面**：name 用小写连字符唯一标识；description 最关键，决定是否加载；好的 description 列举触发短语、定义时序位置、包含产品关键词；扩展字段如 references、allowed-tools、type、estimated_time 可选用
- **03 五种核心模式**：线性流程：明确步骤操作，含 Prerequisites、Quick Start、Fallback、Troubleshooting；决策树+按需加载：大平台选型，主文件精简、references 渐进式披露；循环迭代：TDD 铁律+Red-Green-Refactor+借口反驳表+验证清单；接力棒循环：文件即状态，next-prompt.md 跨 session 持久化；多阶段+检查点：统一阶段模板+Go/No-Go 决策+子 Skill 编排；思维框架：控制思维质量，用量化阈值与非目标约束防幻觉
- **04 通用写作技巧**：防偷懒四武器：强硬语气、借口反驳表、量化阈值、负面指令；教学三方式：Good/Bad 对比、具体命令、完整示例；安全三原则：安全默认值、权限最小化、人类兜底；三层知识组织：frontmatter ~100 tokens、主文件 2K-5K、references 按需加载
- **05 模式选择决策树**：按六个问题分支：明确步骤→线性；大量选项→决策树；单会话循环→迭代；跨 session 长项目→接力棒；多天多周 Go/No-Go→多阶段；深度分析→思维框架
- **06 快速上手模板**：线性模板：frontmatter + Prerequisites + Steps + Troubleshooting；循环迭代模板：Core Principle + The Loop + Rationalizations + Completion Checklist
- **07 参考资源**：官方规范：agentskills.io 与 anthropics/skills 的 template、spec；精选仓库：openai/skills、obra/superpowers、stitch-skills 等；精选列表：awesome-agent-skills 500+ 索引、awesome-claude-skills 含 Skill vs MCP 对比
- **08 7 个 Skill 速查表**：7 个 Skill 覆盖线性到思维框架，行数 77–502 不等；vercel-deploy 是最小完整模板，discovery-process 编排 10+ 子 Skill

### 带走三句话

1. 按决策树选模式：线性/决策树/迭代/接力棒/多阶段/思维框架
2. 三层知识组织控 token：frontmatter 精简、主文件 2K-5K、参考按需加载
3. 写 Skill 要用强硬语气、借口反驳表与负面指令防 LLM 偷懒

### 知识卡

跨全文挖掘 11 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
