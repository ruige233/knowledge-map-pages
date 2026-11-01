---
title: "如何写好 Skill：一份终极实战经验手册"
url: "https://mp.weixin.qq.com/s?__biz=MjM5ODYwMjI2MA==&mid=2649801836&idx=1&sn=f01a69e0c012eafde5f8bc76804bc5fb"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent框架, Skill]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/如何写好 Skill：一份终极实战经验手册.html"
---

# 如何写好 Skill：一份终极实战经验手册

> 一句话定位：从定义到评估，系统讲透如何写出好用可靠的 AI Skill

## 摘要

本文是作者结合踩坑经验与 Anthropic 官方实践整理的 Skill 写作手册。文章先厘清 Skill 的定义、价值与三层渐进式加载机制，再给出目录结构、SKILL.md 模板与 Quick Start。核心技法包括写准 Description、开头讲清目标与适用判断、用祈使句并解释原因、提供 Before/After 与 Few-Shot 示例、用表格和流程图表达分支。文章还覆盖模块化拆分、MCP 与 HTTP 集成选型、安全底线、Skill Creator 工程化评估、生命周期与反模式。

## 要点

- Skill 是结构化 Prompt Engineering：通过 SKILL.md 把领域知识、流程固化为 AI 可执行指令集。
- 三层渐进式加载控制成本：Level 1 元数据常驻、Level 2 正文触发时加载、Level 3 资源按需引用。
- Description 决定触发质量：太笼统或太窄都不行，可用 20 问触发评估校准命中率。
- 正文写法四要素：祈使句下指令并解释原因、Before/After 对比、Few-Shot 示例、表格/流程图可视化。
- 长 Skill 要模块化：超 500 行拆成主 Skill + 子 Skill，单一职责、写明依赖、可独立使用。
- 外部服务按需选 MCP 或 HTTP 脚本，并守住密钥、危险操作、Prompt 注入三条安全底线。
- 用 Skill Creator 工程化评估触发准确率、召回率与效果通过率，用数据迭代优化。
- 完成前过检查清单并掌握调试：多数问题出在未加载成功或 Description 不匹配。

## 提炼

> 一句话定位：从定义到评估，系统讲透如何写出好用可靠的 AI Skill

### 分节提炼

- **01 什么是 Skill**：Skill 是结构化 Prompt Engineering，把知识流程固化为可执行指令集。；三层加载：Level 1 元数据常驻、Level 2 触发加载、Level 3 按需引用。；核心原则：Level 1 越精准越好，Level 2 越精简越好，Level 3 放心放。；Rule 是全局底线，Skill 是按需触发的能力包，两者用途不同。
- **02 Skill 的结构**：最小 Skill 是文件夹 + SKILL.md，复杂场景加 scripts/references。；SKILL.md 由 YAML 头与 Markdown 正文组成。；放用户级目录全局生效，放项目级目录仅当前项目生效。；Quick Start：建目录写 YAML 与正文，再用对话测试自动触发。
- **03 关键写作技巧**：Description 要精准：太笼统该触发不触发，太窄漏场景，可用 20 问触发评估校准。；开头讲清做什么、为什么、怎么做判断，并给出跳过条件。；用祈使句直接下指令，并解释原因而非堆 MUST。；关键操作给 Before/After 对比，再放 3-5 个 Few-Shot 示例。
- **04 模块化拆分**：超过 500 行、含多个独立流程、部分可单独用时该拆分。；复杂场景拆成主 Skill（编排）+ 子 Skill（可独立调用），主文档按顺序引用 steps/。；子 Skill 单一职责、写明依赖关系、每个都能单独使用。
- **05 进阶写法**：能结构化的信息尽量用表格，AI 读表比读长文准确。；复杂检查逻辑写成脚本放 scripts/，SKILL.md 直接调用。；同一目标给多种方案，并标注易错点与 FAQ。
- **06 MCP 与 HTTP 选择**：MCP 是 AI 专用工具协议（JSON-RPC），HTTP 是通用协议，二者互补。；选型看是否已有 MCP、是否需多平台复用、是否需统一鉴权、是否一次性调用。；简单公开 API、老系统、无复用需求可直接 HTTP 脚本；MCP 管连接、Skill 管流程。
- **07 安全底线**：脚本会被真实执行：绝不硬编码密钥，用环境变量或 .gitignore 配置文件管理。；危险操作必须加确认，数据库操作先备份再改。；防范 Prompt 注入：外部数据当数据不当指令，校验文件名、路径和变量。
- **08 Skill Creator 与评估**：流程是先生成草稿、对比测试、反馈迭代，再工程化评估，约 30-50 分钟出可用 Skill。；触发评估算 Precision/Recall，效果评估对比有无 Skill 的通过率与 Token。；评估纳入维护：放 evaluation/，改 description 重跑触发，改步骤重跑效果。
- **09 验证方法**：每个 Skill 配验证清单：功能、构建、运行三类逐项勾选。；配可直接复制的验证命令：grep 残留、go vet、go test、go build。；评估看触发准确率、误报率、输出一致性、Token 效率、完成准确率，重点抓前两项。
- **10 调试排错**：未触发四步排查：是否加载、description 匹配、被抢触发、提问模糊。；多 Skill 冲突用差异化 description、合并、排斥说明或手动指定解决。；执行跑偏就补示例、消矛盾、加检查点；脚本报错查权限、shebang、依赖、路径。
- **11 生命周期管理**：metadata 标版本号，按改动大小走 1.1/1.2/2.0 升级。；可复用 Skill 同步到用户级目录；协作走 PR Review 与 CHANGELOG。；持续迭代：记录问题、找规律、提炼通用规则、精简内容、解释原因。
- **12 常见反模式**：六大反模式：大杂烩、黑话、无示例、无验证点、写死数值、当 Wiki 写。；一 Skill 管一件事；description 用通用语言加技术关键词。；给判断规则和参考范围而非写死数值；背景资料放 references/ 目录。
- **13 核心检查清单**：检查内容：目标明确、Description 精准、语气直接、讲原因、有示例、覆盖场景、标坑、可验证。；检查结构与工程：YAML 完整、正文不超过 500 行、模块化、依赖明确、版本控制、团队协作。；检查安全与可维护性：无硬编码密钥、危险操作有确认、输入有校验、跨平台兼容。
- **14 术语速查**：术语速查覆盖 Level 1/2/3、Few-Shot、Before/After、MCP 等。；术语标注首次出现章节，便于回查相关讲解。
- **15 参考资源**：汇总 Anthropic 仓库、Skill Creator、OpenSkills 链接。；含 MCP Server 列表与 OpenAI Tokenizer 在线工具，便于实践取用。

### 带走三句话

1. 把 Skill 当结构化 Prompt 工程：Description 决定触发，正文用祈使句加示例让 AI 照着干。
2. 控制上下文成本：Level 2 正文精简，超 500 行拆成主 Skill 加可独立复用的子 Skill。
3. 发布前跑工程化评估和检查清单，守住密钥、危险操作与 Prompt 注入三条安全底线。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
