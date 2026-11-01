---
title: "35个 Claude Code 实用技巧！每条都有具体命令或可直接复制的 prompt"
url: "https://mp.weixin.qq.com/s?__biz=MzIwNjg4MzY4NA==&mid=2247544032&idx=1&sn=64fa4275abfa05d29be10200dfe53683"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI编程"
tags: [AI编程, Claude Code, Prompt]
lang: zh
status: ok
阅读页: "阅读页/AI编程/35个 Claude Code 实用技巧！每条都有具体命令或可直接复制的 prompt.html"
---

# 35个 Claude Code 实用技巧！每条都有具体命令或可直接复制的 prompt

> 一句话定位：35个Claude Code实战技巧：命令与prompt速查

## 摘要

文章围绕 Claude Code 的 35 个实战技巧，按项目启动、会话管理、代码质量、探索代码库、架构重构、API 与安全、工程自动化、文档测试、依赖管理、Debug 排查、恢复模式十一个环节展开，每一条都给出可直接执行的命令或 prompt。用 /init 与 /memory 搭底座，Plan 模式先规划后动手；用 /compact、/clear 管理上下文，Opus 规划、Sonnet 实现；靠参考文件、截图调试、测试先行与增量构建保证质量；用架构审计、API 审查、安全扫描与性能分析守住设计与安全；再用 Git Hook、CI 与 Release Notes 自动化，最后以 Git Checkpoint 兜底、Recovery 模式止损。

## 要点

- /init 扫描代码库生成 CLAUDE.md，新会话自动读取项目背景
- Plan 模式（Shift+Tab）先规划再实现，是防 bug 最有效的习惯
- /compact、/clear 管理上下文，Opus 做规划、Sonnet 做实现
- 参考文件、截图调试、测试先行、增量构建提升代码质量
- 架构审计、重构规划与数据库迁移让多层联动改动可落地
- API 审查、安全扫描与性能分析覆盖设计与安全
- Git Hook、CI、环境脚本、Release Notes 实现工程自动化
- Debug 贴完整错误、Git Checkpoint 兜底、Recovery 模式止损

## 提炼

> 一句话定位：35个Claude Code实战技巧：命令与prompt速查

### 分节提炼

- **01 项目启动四步**：/init 扫描代码库生成 CLAUDE.md，记录结构、技术栈与规范，新会话自动读取；/memory 写入跨会话持久规则，如 TypeScript strict、公共函数加 JSDoc；Plan 模式（Shift+Tab）只分析不写代码，确认后再切回实现；在 CLAUDE.md 里用参考文件锁定新建文件的 API、repository、组件风格
- **02 会话管理**：/compact 在聊 30-45 分钟后压缩上下文，保留关键决策与当前状态；/clear 开新任务时清空上下文，一个功能一次对话；/cost 每小时查看 token 用量，设定心理预算；Opus 做规划与架构决策，Sonnet 做具体实现；! 前缀直接跑终端命令
- **03 代码质量**：参考文件法：直接指文件比口头描述风格准确十倍；UI 问题截图 Ctrl+V 粘贴，比文字描述快且准；先写测试定义行为再实现，覆盖正常、边界与异常输入；增量构建拆成小步骤逐步测试，Diff Review 每次让改动逐处解释
- **04 探索代码库**：不熟悉的模块动手前先提问，解释数据流向、用到的模式与注意点；先理解架构再动手，避免引入与现有设计冲突的实现
- **05 架构与重构**：架构审计：提出两种方案，给组件图、优缺点、复杂度与风险，再推荐一种；重构规划：给出新文件结构、迁移内容，并确保不破坏外部引用；数据库迁移可让 Claude Code 联动更新迁移文件、repository、API 与类型定义
- **06 API与安全**：API Review 检查命名一致性、错误响应、分页、鉴权与 REST 规范；安全扫描覆盖 SQL 注入、XSS、secrets、缺输入校验、IDOR、缺限流；性能分析查 N+1、缺索引、不必要重渲染、大包与可缓存接口，按影响排优先级
- **07 工程自动化**：pre-commit hook 对暂存文件跑 lint、类型检查并阻止 console.log 提交；GitHub Actions 在每次 PR 跑依赖安装、测试、lint、构建并评论结果；环境搭建脚本一键装依赖、建 .env、建库、迁移、灌数据、跑测试；Release Notes 按新功能、bug 修复、性能、Breaking Changes 分类生成
- **08 文档与测试数据**：功能做完立刻生成文档，比几天后靠记忆写准确；测试数据要真实并覆盖边界：归档项目、已删除用户、无成员项目
- **09 依赖管理**：加依赖前检查维护状态、已知安全问题、bundle 体积与更轻量替代方案；依赖冲突要分析原因，给出改动最少的方案并说明 tradeoff
- **10 Debug排查**：出错贴完整错误与堆栈，并要求“逐步分析根本原因”；大改动前 git commit checkpoint，出问题秒级回滚；Bug 走复现步骤→失败测试→修代码一条线，Blame 调查找出引入 commit
- **11 恢复模式**：坏实现来回改无效时止损，读 git 原始可用版本并重述目标重来；从干净状态重新开始比修补一堆错误快，知道何时止损是重要能力

### 带走三句话

1. 打牢底座：/init 生成 CLAUDE.md，/memory 写持久规则，Plan 模式先规划后实现。
2. 上下文即质量：/compact 定时压缩，/clear 一个功能一次对话，Opus 规划 Sonnet 施工。
3. 质量兜底：测试先行加增量构建，Git Checkpoint 秒回滚，不行就 Recovery 止损。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
