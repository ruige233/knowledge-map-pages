---
title: "逆向深扒Claude Code源码，我发现了什么！？"
url: "https://mp.weixin.qq.com/s?__biz=MzI2NDU4OTExOQ%3D%3D&mid=2247695110&idx=1&sn=894062c617d73eeda69d379af3a2ea32"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI编程"
tags: [AI编程, Claude Code]
lang: zh
status: ok
阅读页: "阅读页/AI编程/逆向深扒Claude Code源码，我发现了什么！？.html"
---

# 逆向深扒Claude Code源码，我发现了什么！？

> 一句话定位：逆向解析Claude Code源码：12层渐进式包装打造工业级编码代理

## 摘要

本文基于泄露的 Claude Code v2.1.88 源码（约1884个TypeScript文件、51.2万行代码、12MB bundle）做逆向分析。核心结论：其强大不在单一技术突破，而在12层渐进式工程包装把一个最小Agent Loop升级为工业级自主编码代理。文章依次剖析架构全景、while-true核心循环、12层包装机制、工具系统、Prompt分层缓存、权限纵深、上下文三重压缩、子代理与Swarm、Skills按需注入、会话持久化、MCP集成、编译时特征门控与未来路线图，并总结开闭原则、fail-closed默认值等设计模式，给出构建AI Coding Agent的优先级清单。

## 要点

- Claude Code 的核心是一个极简 while-true 循环，复杂性被推到工具层与12层包装层。
- query.ts 约785KB，是整个 Agent Loop 的心脏，胖核心设计刻意保持循环的原子性。
- 12层包装机制从Planning到Worktree隔离逐层叠加，仅加Planning就让任务完成率翻倍。
- buildTool() 工厂采用 fail-closed 默认值，新工具默认串行、非只读、需权限。
- System prompt 用动态边界标记拆分静态前缀与动态后缀，靠 Blake2b 哈希前缀缓存省 token。
- 上下文管理用 autoCompact/snipCompact/contextCollapse 三层策略做透明压缩。
- Fork 子代理的输出不进主代理上下文，避免上下文污染；Swarm 队友自主认领任务。
- 编译时 feature() 门控配合 DCE 让内部功能在发布版被删除，外部版本只是精简版。

## 提炼

> 一句话定位：逆向解析Claude Code源码：12层渐进式包装打造工业级编码代理

### 分节提炼

- **01 总览**：核心竞争力来自工程化、Agent架构、Skills系统三个维度；12层渐进式包装是全文核心工程洞察
- **02 架构全景**：query.ts约785KB是Agent Loop心脏，刻意胖核心；集中协调流式处理、工具调度、压缩与子代理管理
- **03 核心Agent Loop**：核心是极简while-true循环且从不改变；复杂性被推到工具层和包装层；加新功能=加新工具或新包装层，体现开闭原则
- **04 12层包装机制**：s01到s12从Agent Loop到Worktree隔离逐层叠加；Planning仅靠先列步骤再执行就让完成率翻倍
- **05 工具系统**：buildTool()提供统一生命周期接口；fail-closed默认值保证新工具默认安全；StreamingToolExecutor按isConcurrencySafe分区并行/串行
- **06 Prompt工程**：system prompt拆静态前缀与动态后缀，靠边界标记做前缀缓存；用prompt约束对抗LLM过度工程化；内部用户获得更严格指令，外部版本并非最优
- **07 权限系统**：四层防御纵深配合Hook、Pattern Matching、Session Memory；YOLO模式跳过检查但有独立安全分类器兜底
- **08 上下文管理**：autoCompact/snipCompact/contextCollapse三层策略；FRC清除旧工具结果并提示模型提前记笔记
- **09 子代理与多代理**：default/fork/worktree/remote四种Spawn模式；Fork输出不进主上下文避免污染；Swarm队友自主认领任务，AsyncLocalStorage隔离上下文
- **10 Skills系统**：通过tool_result按需注入而非预加载；CLAUDE.md按目录惰性加载；EXPERIMENTAL_SKILL_SEARCH预示Skill市场
- **11 会话持久化**：JSONL追加日志，user阻塞写、assistant fire-and-forget；支持--continue/--resume/--fork-session恢复
- **12 MCP集成**：支持stdio/sse/http/ws/sdk多种传输；MCP工具与内置工具在权限与渲染中统一处理
- **13 编译时特征门控**：feature()是编译时内在函数，内部true外部false；DCE移除未启用代码；GrowthBook用随机词对混淆flag用途
- **14 未来路线图**：Numbat下一代模型、Opus 4.7、Sonnet 4.8开发中；KAIROS全自主代理模式是最大未发布特性
- **15 设计模式与哲学**：11种设计模式如AsyncGenerator流式、Ring Buffer；7条工程哲学：fail closed、measure twice等
- **16 对我们的启示**：P0先做Agent Loop+Bash与Planning；按需注入Skills、分层缓存、压缩按优先级推进；三级跳：CLI Tool到Agent Product再到Autonomous Platform

### 带走三句话

1. 做一个能用的Coding Agent，先实现极简Agent Loop+Bash，再叠加Planning，别一上来堆架构。
2. 安全默认值比事后检查更重要：fail-closed、量两次切一次，把权限与可逆性做进工具工厂。
3. 知识按需注入、上下文透明压缩、静态前缀缓存，这些工程细节决定Agent的成本与体验。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
