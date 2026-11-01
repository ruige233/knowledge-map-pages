---
title: "从Vibe Coding到Agentic Engineering：重构后台开发全流程"
url: "https://mp.weixin.qq.com/s?__biz=MjM5ODYwMjI2MA==&mid=2649801233&idx=1&sn=6e6eaf8a0e94e8349bbbef4b734dc5c0"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI编程"
tags: [AI编程, Agent]
lang: zh
status: ok
阅读页: "阅读页/AI编程/从Vibe Coding到Agentic Engineering：重构后台开发全流程.html"
---

# 从Vibe Coding到Agentic Engineering：重构后台开发全流程

> 一句话定位：Claude Code 实践：需求到发布全流程串进一个终端会话

## 摘要

本文是作者用 Claude Code 加自定义 Skill/Command/MCP 体系，把后台开发从需求到发布全流程串进一个终端会话的实践。核心对比 Vibe Coding 的“提示即祈祷”与 Agentic Engineering 的流程化：人定义目标、审核方案、把关质量，AI 在结构化流程中自主执行规划、编码、测试与迭代。文章以 RedeemReward 上报逻辑变更为例，走通需求创建到合入发布共 11 个阶段，并给出三层工具体系、各阶段耗时与关键经验。

## 要点

- Vibe Coding 是“提示即祈祷”，生成代码质量不可控且缺审查流程；Agentic Engineering 用结构化流程和人工审核节点约束 AI。
- 工具链分三层：Skill 承载业务逻辑、Command 做斜杠入口、MCP 连接外部平台 API。
- 全流程 11 个阶段从需求创建到合入发布，开发者角色从“亲自执行”变为“审核确认”。
- 需求澄清先读代码再提问再出方案，brainstorming 产出共识设计，writing-plans 生成含精确路径与验证标准的计划。
- 并行开发用子 Agent 执行各 Task，自动跑 spec review 和 code quality review，并生成 Conventional Commits。
- 代码自审用 code-review 按 Critical/Major/Minor/Suggestion 分级审查，提 MR 前清掉低级问题。
- AI 评审能区分“本次引入”与“历史遗留”，但意见有对有错必须人工把关；跨模型交叉审查可减少盲区。
- 合入发布仍保持纯人工，涉及灰度策略与线上风险暂不交给 AI。

## 提炼

> 一句话定位：Claude Code 实践：需求到发布全流程串进一个终端会话

### 分节提炼

- **01 引言**：Vibe Coding 是“提示即祈祷”，生成代码质量不可控、缺审查流程；Agentic Engineering 让 AI 在结构化流程中执行、每个关键节点有人工审核
- **02 全流程概览**：从需求到发布共 11 个阶段，开发者角色从亲自执行变成审核确认；各阶段有明确核心工具与人工操作点
- **03 工具体系速览**：Skill 承载核心逻辑并自动触发，Command 是 /xxx 入口，MCP 连接外部平台 API；superpowers 插件提供结构化工作流 Skill，防止 AI 跳过关键步骤自由发挥
- **04 需求获取与分支**：pm-dev 支持给 PM URL 解析需求或口述需求自动创建需求单；创建后自动建 feature/{username}_{short_id} 分支并保存需求文档；所有 Skill 都支持 /skill-name 显式调用
- **05 交互式需求澄清**：AI 先探索代码现状再提问，逐步明确需求边界；brainstorming 产出双方共识的设计方案并保存 commit；不要重复造轮子：编排串联现有 Skill 优于自研全部能力
- **06 制定实施计划**：writing-plans 深入读代码细节，确保计划有精确路径、变更描述和验证标准；计划拆成多 Task 并自动保存 commit，人工审核通过才进入执行
- **07 并行执行开发**：Subagent-Driven 为每个 Task 派子 Agent 并行执行；每个 Task 完成后自动跑 spec review 与 code quality review；/commit 自动生成 Conventional Commits 格式提交
- **08 代码审查自审**：code-review 按 Critical/Major/Minor/Suggestion 四级审查；自审定位是提 MR 前清掉格式命名规范类低级问题，与阶段 9 正式评审定位不同
- **09 编译部署**：dtools 支持 dpatch/bpatch/ipatch 三种发布模式；自动处理 Mac→Linux 交叉编译
- **10 日志排查调试**：galileo-log-query 支持按 target/时间/traceID 查日志；内置 limit 50 防上下文爆炸，查不到时降级到 Galileo MCP 问询接口；目前日志排查半自动：测试请求仍需人工触发
- **11 创建 MR**：/create-mr 提取 short_id 关联 PM，分析 commit 生成 MR 标题描述；git-context 提供 project-context 与 mr-locate 两个复用模块
- **12 AI 辅助评审**：/review-mr 仅审查新增/修改行，并用 git show 验证行号准确性；AI 能区分本次引入与历史遗留问题，避免提历史债务；AI 评审有对有错，每条意见必须人工判断；跨模型交叉审查可避免同一模型盲区
- **13 修复评审意见**：/fix-mr 拉取全部未解决评论，读前后 10 行上下文分析并生成修复方案；修改后自动 go build ./... 验证并逐条回复评论
- **14 合入发布**：CI/CD 自动编译、构建镜像、灰度发布；合入与发布目前纯人工，不交给 AI
- **15 总结**：人是编排者定义目标与把关，AI 在结构化流程中自主执行；Agentic Engineering 依赖流程，Vibe Coding 依赖运气；各阶段实际耗时多在 2-10 分钟，人工只做确认类操作
- **16 附录**：工具速查表覆盖 Skill/Command 与对应阶段；复杂 MR 案例中 AI 审查出 9 个问题，reviewer 调整后提交 7 条；配置集中在 ~/.claude-internal 目录

### 带走三句话

1. 把 AI 的能力装进有纪律的流程，而不是让 AI 自由发挥后祈祷结果正确。
2. 每个关键节点都留人工审核：计划、部署、评审意见、合入发布。
3. 自研 Skill 的价值在编排串联，跨模型交叉审查能补上单一模型的盲区。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
