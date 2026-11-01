---
title: "一文搞懂Hermes：新顶流Agent如何从经验中自我进化"
url: "https://mp.weixin.qq.com/s?__biz=MzI2NDU4OTExOQ==&mid=2247695259&idx=1&sn=30ef327bcbbb3e349abd812da627c5f2"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent框架, Agent]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/一文搞懂Hermes：新顶流Agent如何从经验中自我进化.html"
---

# 一文搞懂Hermes：新顶流Agent如何从经验中自我进化

> 一句话定位：源码级拆解Hermes Skills闭环：经验提取、条件激活与自改进

## 摘要

Hermes Agent 的核心创新是 Skills 闭环系统，打通经验提取→知识存储→智能检索→上下文注入→执行验证→自动改进，是开源框架中唯一内置闭环自学习的项目。从源码拆解：Agent 按 System Prompt 触发条件自主创建 Skill，创建经七道安全关卡；索引用 LRU 与磁盘快照两层缓存；条件激活与渐进式披露控制 token 开销；内容以 User Message 注入保护 Prompt Cache；Fuzzy Match 驱动自改进；最后指出缺版本控制等改进空间。

## 要点

- Hermes Agent（GitHub 71.8K Stars）核心创新是 Skills 闭环系统，在主流开源 Agent 框架中唯一内置闭环自学习。
- Agent 依据 System Prompt 中的创建触发条件自主决定何时建 Skill（复杂任务 5+ 次工具调用、踩坑、非平凡流程）。
- Skill 创建经过七道安全关卡：名称/分类/Frontmatter/大小/冲突/原子写入/安全扫描。
- 索引构建采用两层缓存：进程内 LRU（8 条）+ 磁盘快照（mtime+size manifest 校验），热路径约 0.001ms。
- 条件激活按 fallback_for/requires_toolsets 等元数据控制 Skill 可见性，避免索引膨胀。
- 渐进式披露三级加载：索引→skill_view 全文→支撑文件，百个 Skill 仅增约 2000 tokens。
- Skill 内容作为 User Message 注入而非修改 System Prompt，以保护 Prompt Cache、节省 90%+ 成本。
- 自改进用 Fuzzy Match 引擎 patch Skill，并触发缓存清理实现最终一致性；skills_guard 静态扫描防御恶意 Skill。

## 提炼

> 一句话定位：源码级拆解Hermes Skills闭环：经验提取、条件激活与自改进

### 分节提炼

- **01 引言**：普通 Agent 无跨会话记忆，每次对话都是白纸；Hermes 会把成功流程提炼为 Skill 并在下次加载复用；Skill 含触发条件、执行步骤、陷阱与验证方法
- **02 全局视角**：七个模块覆盖经验提取到知识复用完整生命周期；闭环为经验提取→知识存储→智能检索→上下文注入→执行验证→自动改进；本质是像专家一样把成功做法写成 SOP 并持续修订
- **03 Skill 创建**：Agent 自主决定何时创建 Skill，无需用户要求；触发条件：复杂任务 5+ 次工具调用、修复棘手错误、发现非平凡流程；创建经七道关卡：名称/分类/Frontmatter/大小/冲突/原子写入/安全扫描；Skill 为 YAML Frontmatter + Markdown Body 格式
- **04 索引构建**：每次启动扫描文件系统开销大，尤其多用户 Gateway 场景；两层缓存：进程内 LRU（8 条）+ 磁盘快照；磁盘快照用 mtime+size manifest 校验有效性；命中耗时：Layer1 ~0.001ms，Layer2 ~1ms，全扫描 50-500ms
- **05 条件激活**：frontmatter 元数据驱动 Skill 可见性；fallback_for_toolsets 在主工具可用时隐藏冗余 Skill；requires_toolsets 在依赖缺失时隐藏 Skill；platforms 字段按操作系统过滤
- **06 渐进式加载**：Token 就是钱，全量塞 System Prompt 可能 100K+ tokens；三级披露：索引→skill_view 全文→支撑文件；100 个 Skill 只增约 2000 tokens；skill_view 含 Prompt Injection 检测、路径穿越防护、环境变量检查
- **07 注入策略**：Skill 内容作为 User Message 注入而非修改 System Prompt；为保护 Prompt Cache，节省 90%+ token 成本；用 [SYSTEM:] 前缀模拟系统级权威；牺牲少量指令跟随权重换取数十倍成本节约
- **08 自改进机制**：System Prompt 要求发现 Skill 过时立即 patch，不等用户要求；patch 复用文件编辑的 Fuzzy Match 引擎；Fuzzy Match 处理空白、缩进、转义、块锚差异；patch 成功后清理 LRU 与磁盘快照缓存，形成最终一致性
- **09 安全扫描**：90+ 威胁正则模式覆盖密钥外泄、越狱、隐形字符等；信任分级：builtin/trusted/community/agent-created 策略不同；结构检查：文件数≤50、总大小≤1MB、单文件≤256KB；符号链接逃逸检测防止越出 Skill 目录
- **10 Skill 与 Memory**：Memory 存持久事实：用户偏好、环境细节、工具怪癖；Skill 存可复用方法：解决过的问题、新工作流；Memory 回答"是什么"，Skill 回答"怎么做"
- **11 与 Voyager 对照**：Voyager 提出 Skill Library 概念，在 Minecraft 中积累技能；Voyager 是学术原型，Hermes 做完整工程化落地；工程化需解决并发、成本、恶意输入、缓存一致性等问题
- **12 设计权衡**：User Message 注入换 90%+ 成本节约，代价是指令跟随略低；写入后扫描避免 TOCTOU，但需回滚机制；缺版本控制：patch 后旧版本永久丢失；正则扫描可被 Base64、变量间接引用、同形字绕过
- **13 总结**：Skills 闭环是程序性记忆的工程化模拟；五阶段：编码、存储、检索、巩固、迁移；尚未完美：无版本控制、扫描可绕过、匹配可能遗漏

### 带走三句话

1. Skill 系统让 Agent 把成功经验沉淀为可复用、可迭代的 SOP，这是 Hermes 最核心的差异化能力。
2. 工程细节决定成败：原子写入、两层缓存、User Message 注入、Fuzzy Match，都为可靠性服务。
3. 闭环尚未完美：缺版本控制、正则扫描可绕过、匹配靠 LLM 判断，值得工程上继续补强。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
