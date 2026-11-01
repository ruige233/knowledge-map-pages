---
title: "当整个团队开始 0 人工Coding：一份万字AI Native研发实战手册"
url: "https://mp.weixin.qq.com/s/Tq0sW8uorXh0kfcNF3blXg"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "其他"
tags: [其他]
lang: zh
status: ok
阅读页: "阅读页/其他/当整个团队开始 0 人工Coding：一份万字AI Native研发实战手册.html"
---

# 当整个团队开始 0 人工Coding：一份万字AI Native研发实战手册

> 一句话定位：OpenSpec+CodeBuddy：把AI当施工队，实现0人工Coding

## 摘要

本文记录一支研发团队从 2023 年用 AI 辅助到 2026 年全面转向 AI Native 研发的实践。作者指出"把 AI 当打字员"天花板很低，痛点在于人机协作无标准、流程断点、上下文缺失、文档脱节。团队以 OpenSpec 作为机器与人共读的研发契约，配合 CodeBuddy 的 propose/apply/archive 三步指令，把研发流程压缩为"先规划、按图施工、归档同步"，人只负责决策、审批、把关。文章详解三大武器库（知识库、MCP、Skills）如何让 AI 懂项目，并以 openspec-installer 为例解剖生产级 Skill 的构造：SOP、幂等脚本、Bridge Rule、Token 安全防护、三级降级策略。最后给出上手指南、扩展工作流与团队协同规范。

## 要点

- 把 AI 当"打字员"天花板很低，当"施工队"才有真正的效率革命。
- 四大痛点：人机协作无标准、流程断点、上下文缺失、文档脱节。
- OpenSpec 是机器与人共读的"研发契约"，先画清唯一图纸再施工。
- 核心三步指令 /opsx:propose → /opsx:apply → /opsx:archive 压缩代码开发流程。
- AI Native 下人的角色收敛为决策、审批、把关。
- 知识库、MCP、Skills 三大机制分别让 AI 知道、连接、掌握项目。
- openspec-installer 展示生产级 Skill 做法：SOP、幂等、Bridge Rule、安全防护、降级策略。
- 团队协同规矩：原子化变更、tasks.md 不超过 15 项、规范即文档、MR 双重视角。

## 提炼

> 一句话定位：OpenSpec+CodeBuddy：把AI当施工队，实现0人工Coding

### 分节提炼

- **01 写在前面**：AI辅助编码常陷"对话-调试-再对话"循环；2025年已验证AI Coding可行，共识是把AI当施工队；2026年初定下"0 人工 Coding"目标
- **02 辅助编码差在哪**：AI老手辅助编码提效也难超50%；四大痛点：协作无标准、流程断点、上下文缺失、文档脱节；根因是人与AI交互缺乏规范，输出质量不可控
- **03 OpenSpec全链路方案**：OpenSpec是机器与人共读的"研发契约"；先画清唯一标准图纸，AI施工队照图自动施工；人只保留决策、审批、把关三个角色
- **04 0-Coding落地实践**：propose先规划再动手，生成proposal/design/tasks/specs；apply按tasks清单跨文件编码，人只做Code Review；archive合并规范增量，文档与代码永远同步；前端需加原型审查，避免UI返工
- **05 三大武器库**：知识库让AI"知道"项目，分specs核心记忆与MCP外部记忆；MCP让AI"连接"内部工具，信息零损耗传递；Skills把团队SOP封装成可复用技能包
- **06 解剖openspec-installer**：SKILL.md定义600+行SOP，步骤幂等可随时重来；Bridge Rule指路AI读config.yaml，alwaysApply:true保证100%生效；Token交互放指令层，.mcp.json自动入.gitignore防泄漏；版本检查三级降级，辅助功能失败不阻塞安装
- **07 上手指南**：一条curl安装Skill，对话初始化项目，约10分钟搭环境；CLI主攻核心指令，IDE辅助审查文档与解决冲突；父目录启动CLI实现前后端多项目联动
- **08 扩展工作流**：复杂需求开启扩展命令new/continue/ff/verify；explore只讨论输出文档、绝不改代码
- **09 团队协同规矩**：原子化变更：一次Change一个需求，tasks≤15项；"规范即文档"：禁止绕过OpenSpec手写核心逻辑；MR审查同时看代码与proposal/design/specs
- **10 写在最后**：AI重新定义程序员：价值从"能写代码"转向"能指挥AI"；演进史：从机器码到AI Native，不再关心具体实现
- **11 指令总速查表**：汇总explore/propose/apply/archive及扩展指令的阶段与用途
- **12 项目目录结构**：openspec由config.yaml、specs归档区、changes工作区组成
- **13 常见场景速查**：给出新项目配置、新需求、前后端联动等场景的标准操作

### 带走三句话

1. 用 OpenSpec 把需求先落成 proposal/design/tasks 再编码，比提示词技巧更能稳定产出。
2. 让 AI 懂项目靠三件事：知识库给记忆、MCP 给连接、Skills 给方法。
3. 生产级 Skill 的底线：SOP 幂等、凭证不落盘、辅助功能失败不阻塞核心流程。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
