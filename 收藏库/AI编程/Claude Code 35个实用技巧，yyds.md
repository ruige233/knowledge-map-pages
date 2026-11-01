---
title: "Claude Code 35个实用技巧，yyds"
url: "https://mp.weixin.qq.com/s?__biz=MzkwNjMwMTgzMQ==&mid=2247538406&idx=1&sn=900cccc9ea20db613f3db7b5869060fd"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI编程"
tags: [AI编程, Claude Code]
lang: zh
status: ok
阅读页: "阅读页/AI编程/Claude Code 35个实用技巧，yyds.html"
---

# Claude Code 35个实用技巧，yyds

> 一句话定位：35个带命令/prompt的 Claude Code 实战技巧合集

## 摘要

本文整理 35 个 Claude Code 实战技巧，每条都给出可直接使用的命令或 prompt。开篇讲项目启动：用 /init 生成 CLAUDE.md、/memory 写持久规则、Plan Mode 先规划后实现、在 CLAUDE.md 里锁定模式风格；会话管理强调 /compact 压缩上下文、/clear 清空、/cost 查花费、Opus 规划 Sonnet 实现、! 前缀跑终端命令与并行 Session。代码质量部分给出参考文件法、截图调试、先测试后实现、增量构建、Diff Review。随后覆盖陌生代码库提问、架构审计与重构、数据库迁移、API 设计与安全扫描、性能分析、Git Hook 与 CI、文档与测试数据生成、依赖检查与冲突解决、Debug 流程与 Git Checkpoint，最后以 Recovery Mode 止损收尾。

## 要点

- /init 生成 CLAUDE.md 记录项目结构、技术栈与编码规范，新会话自动读取
- Plan Mode（Shift + Tab）只分析不写代码，确认方案后再切回实现模式
- /compact 每 30–45 分钟压缩上下文，/clear 遵循一个功能一次对话
- 用参考文件、截图、先测试后实现与增量构建提升代码质量
- 架构审计、API Review、安全扫描、性能分析等 review 类 prompt 可直接套用
- Git Checkpoint 大改动前兜底回滚，Recovery Mode 走不通时止损重来

## 提炼

> 一句话定位：35个带命令/prompt的 Claude Code 实战技巧合集

### 分节提炼

- **01 项目启动**：/init 扫描代码库生成 CLAUDE.md，新会话自动读取项目背景；/memory 写入跨会话持久规则，如 strict 模式、JSDoc 注释；Plan Mode 先分析给方案，确认后再切回实现模式；在 CLAUDE.md 里锁定参考文件路径，统一新建文件风格
- **02 会话管理**：/compact 在 30–45 分钟后压缩对话，保留关键决策与状态；/clear 开新任务清空上下文，遵循一个功能一次对话；/cost 每小时查 token 用量，设定心理预算；Opus 规划、Sonnet 实现；! 前缀直接跑终端命令；大功能用并行 Session
- **03 代码质量**：用参考文件代替文字描述风格，更准确且利于团队一致；UI 问题直接截图粘贴，比文字描述快且准；先写测试用例再实现，测试先行定义行为；大功能拆小步、每步测试；改完用 Diff Review 检查意外改动
- **04 探索陌生代码库**：动手前先让 Claude Code 解释模块数据流与设计模式；先理解架构再动手，避免与现有设计冲突
- **05 架构与重构**：架构审计：提两种方案，给出组件图、优缺点、复杂度与风险；重构规划要说明新文件结构与迁移位置，且先不要开始重构；数据库迁移：列全 schema 改动与所有需要改动的文件
- **06 API 设计与安全**：API Review 检查命名一致性、错误响应、分页、鉴权与 REST 规范；安全扫描覆盖 SQL 注入、XSS、secrets、输入校验、IDOR、限流；性能分析检查 N+1 查询、缺失索引、重渲染、懒加载与缓存
- **07 工程自动化**：pre-commit hook 跑 lint、类型检查并拦截 console.log；GitHub Actions 在 PR 触发测试、lint、构建并评论结果；环境搭建脚本一次跑通，Release Notes 按分类生成
- **08 文档与测试数据**：功能做完立刻生成文档，比事后凭记忆更准确；测试数据要真实，覆盖角色、关联与边界情况
- **09 依赖管理**：加依赖前检查维护状态、安全问题、bundle 体积与替代方案；冲突解决：分析原因，找冲突共享依赖，给改动最少的方案
- **10 Debug 与问题排查**：粘贴完整错误堆栈并约束逐步分析根本原因；大改动前 git commit checkpoint，出问题秒级回滚；Bug 复现走最小步骤 + 失败测试 + 修复；Blame 调查：从 git log 找引入问题的 commit
- **11 恢复模式**：方案走不通时读回 git 里的可用版本，换思路重新开始；及时止损本身就是 AI 辅助开发的重要能力

### 带走三句话

1. 把 /init 生成的 CLAUDE.md 当作项目底座，配合 /memory 让规则跨会话自动生效。
2. Plan Mode 先设计后动手、增量构建每步测试、Git Checkpoint 兜底，三者决定交付质量。
3. 方案走不通时用 Recovery Mode 读回可用版本止损重来，比反复修补更快。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
