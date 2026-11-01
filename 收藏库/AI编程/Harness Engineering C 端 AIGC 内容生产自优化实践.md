---
title: "Harness Engineering C 端 AIGC 内容生产自优化实践"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247560353&idx=1&sn=939768570beeacd145b5c1c91ae7827b"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI编程"
tags: [AI编程, Harness]
lang: zh
status: ok
阅读页: "阅读页/AI编程/Harness Engineering C 端 AIGC 内容生产自优化实践.html"
---

# Harness Engineering C 端 AIGC 内容生产自优化实践

> 一句话定位：蚂蚁保 DIPG：离线 verify 闭环让 C 端 AIGC 页面可交付

## 摘要

蚂蚁保 DIPG 把 Harness 思想搬进 C 端 AIGC 生产链路。核心翻转：不让 LLM 实时直出，改「离线生成→修正闭环→刷入 DB→C 端直出」，实时只兜底。三个 Agent 协作：Research 从零生成，Verify 程序化加 LLM 双层校验，Host 按 fix_hint 精准修正。Verify 经 /audit/ 比对产物与原料抓错误，高频错误再蒸馏回 prompt，形成三级 Harness。

## 要点

- 把实时生成降为兜底，只有通过 verify 的 HTML 才刷入 DB，C 端默认读离线产物。
- 三个 Agent 分工：Research 从零生成、Verify 两层校验、Host 编排并按 fix_hint 精准修正。
- 程序化校验抓结构错误（孤儿闭合标签），LLM 事实校验抓幻觉（无数据来源的对比数字）。
- /audit/ 审计通道让 Verify 对齐 HTML 与生产原料，是事实校验的前提。
- 修正走 Host 自己 patch 而非重派 Research，避免全盘重写。
- Verify 抓到的错误模式蒸馏回 prompt，让一次过概率持续提升（回灌）。
- 三级 Harness：线下迭代 verify、线上离线把关、线上实时兜底，层层递进。
- 四条踩坑经验：不实时直出、双链路同源、确定性判定不留给 LLM、verify 必须看原料。

## 提炼

> 一句话定位：蚂蚁保 DIPG：离线 verify 闭环让 C 端 AIGC 页面可交付

### 分节提炼

- **01 双链路架构**：离线链路是主路径：Research+Host+Verify 闭环，通过才刷入 DB。；实时链路只跑一次 Research Agent，不校验不修正，仅兜底未开启品。；用户请求直接读 DB 离线产物，命中率 100%，缓存只是性能优化。；三 Agent 分工：Host 编排并精准修正、Research 从零生成、Verify 只校验。
- **02 两类致命 badcase**：孤儿 </div> 被移动端容器当作关闭信号，导致兄弟组件渲染错位。；「优于市场 85%」无任何数据来源，是典型幻觉，渲染正常却误导用户。；两类错误分别需要程序化校验和 LLM 事实校验，对应 Verify 的两个节点。；抓到错误后回灌 prompt，让后续生成从源头减少同类错误。
- **03 三层 Agent 结构**：三层 LangGraph：外层硬编码边，中层 Host，内层两个 SubAgent。；task 工具注入子 Agent；新增 SubAgent 只改 blueprint 多挂一个。；每次调用新 thread_id 隔离上下文，只回一条 ToolMessage。；state["files"] 是跨 Agent 虚拟文件系统，files 存产物。
- **04 Prompt 契约**：合规用语硬规则列明敏感词与合规替换，是监管红线。；8 条事实性规则：信源优先级、无数据不展示、禁止盲目对比等。；强制前置溯源：生成关键数据前先写 <!-- Source: ... -->，写不出来就留空。；规则来自合规底线与 Verify 高频错误，清单随回灌持续扩充。
- **05 Verify 闭环**：三阶段闭环：研究生成→验证循环→交付，终止条件是 Verify 不再返回修正意见。；structural_check 纯 Python 校验零假阳性；llm_verify 做事实对齐。；Verify 必须读 /audit/ 原料，对 HTML 数值与数据源做对齐而非静态分析。；修正由 Host 按 fix_hint 精准 patch，不重派 Research，避免全盘重写。
- **06 错误回灌**：把关是直接价值（当次产物不刷库），回灌是间接价值（后续一次过概率更高）。；半自动回灌：把 Verify 高频 issue 抽象成通用规则写进 Research prompt。；实体对齐、禁止盲目对比、严禁额外包裹容器都是回灌产物。；没有回灌，每次都要抓同样的错、反复 patch，成本高且不收敛。
- **07 三级 Harness**：三级 loop 时间尺度差一到两个数量级，共享 Verify 与数据契约。；Level 3 让 verify 更强，Level 2 是主干，Level 1 是兜底。；线下迭代 verify 能力→离线把关主交付→实时靠升级 prompt 兜底，层层喂给下一层。
- **08 踩坑经验**：不要让 LLM 实时产物直出；改为离线生成+把关+持久化直出，实时兜底。；两条链路的生成器代码与 prompt 必须严格同源，改进才能自动传导。；能用确定性程序判定的不要留给 LLM，token 投给语义判断。；verify 必须看得到生产原料，/audit/ 是事实校验前提。
- **09 结语**：概念翻转：C 端 AIGC 默认假设应是离线生成→Harness 把关→持久化产物，实时只兜底。；离线 Harness 有两重价值：不合格不刷库、错误蒸馏回 prompt，缺一质量不可收敛。；模式可迁移到 AI 图表/文档/营销素材等直接面对用户的场景。

### 带走三句话

1. 把实时生成降为兜底：C 端 AIGC 的默认假设应是离线生成→Harness 把关→持久化直出。
2. 事实校验不是读 HTML 措辞，而是拿产物对齐 /audit/ 里的生产原料。
3. Verify 抓到的错要回灌 prompt，否则每次都要抓同样的错、反复 patch，质量不收敛。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
