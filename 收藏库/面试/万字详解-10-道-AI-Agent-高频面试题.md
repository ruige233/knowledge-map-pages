---
title: "万字详解 10 道 AI Agent 高频面试题"
url: "https://mp.weixin.qq.com/s/XmeuMQwiuxfQKMbeZWJfbQ"
source: "微信公众平台 · 楼仔"
published: "2026-10-28"
archived: "2026-10-28"
category: "面试"
tags: [AI Agent, 面试题, Function Calling, MCP, RAG]
lang: zh
status: ok
阅读页: "阅读页/面试/万字详解-10-道-AI-Agent-高频面试题.html"
---

# 万字详解 10 道 AI Agent 高频面试题

> 一句话定位：面向求职与面试复习的 AI Agent 高频考点串讲，用「是什么 → 怎么干 → 怎么变好」三层骨架，覆盖从 Agent 本质、范式选型到 Function Calling、MCP、记忆、上下文管理、RAG 与评估的完整知识面。

## 摘要

以 10 道高频面试题串起 AI Agent 岗位的核心考点：先厘清 Agent 与普通 LLM 应用的本质区别（自主决策、目标驱动、循环闭环）及 Workflow / 单 Agent / 多 Agent 的选型判据；再展开「怎么干」——Function Calling 的六步调用链路、MCP 与其正交关系、工具调用失败的容错与防死循环四道防线、短期/长期记忆与知识库/日志的边界；最后落到「怎么变好」——上下文溢出与腐化的治理（隔离优于压缩、KV Cache 三铁律）、RAG 检索优化（混合检索 + RRF 融合 + 重排 + Agentic RAG）、Agent 评估（Pass@k vs Pass^k、首个错误归因与 Harness/模型分层）。每题配图解、代码与总结，适合应届生与转岗工程师系统复习。

## 要点

- Agent 与普通 LLM 的本质区别：自主决策、目标驱动、循环闭环；循环是自回归生成机制决定的。
- 选型判据：路径写死用 Workflow、需自主决策用单 Agent、拆多 Agent 前先问「有没有新信息」。
- Function Calling 铁律：模型只做决策不执行，产出的是调用意图 JSON；Pydantic 校验发生在返回参数之后。
- MCP 与 FC 正交：FC 管模型侧表达调用，MCP 管工具侧标准化接入；工具多导致上下文爆炸时看路由策略。
- 防跑飞四道防线：轮次上限、完成标记、结果校验、人工介入；框架 recursion_limit 兜底。
- 记忆判据：会被语义检索出来并影响下一次决策的才算记忆；冲突处理追加比删除安全。
- 溢出（装不下）vs 腐化（装得下但找不到）：隔离优于压缩；KV Cache 三铁律（稳定前缀、动态信息追加末尾、标准 API 格式）。
- RAG 检索优化：向量 + BM25 混合检索 + RRF 融合 + 跨编码器重排；Agentic RAG 按需决定查不查。
- 评估：Pass@k 看能力上限，Pass^k 看业务可靠性；失败归因要归「首个错误」并分清 Harness 还是模型。

## 提炼

> 一句话定位：万字拆解 AI Agent 10 道高频面试题：原理、选型、工具、记忆、评估。

### 分节提炼

- **Q01 Agent 与普通 LLM**：本质区别在有无自主决策与闭环；循环来自自回归机制；权重冻结不自我训练。
- **Q02 Workflow/单 Agent/多 Agent**：路径写死用 Workflow，写不死上单 Agent；拆多 Agent 前问「有没有新信息」。
- **Q03 四大推理范式**：CoT 只推理、ReAct 推理+行动、Plan 先全局、Refine 事后审；生产混用。
- **Q04 Function Calling**：模型只决策不执行；Pydantic 管格式，业务合法性在函数内再验。
- **Q05 MCP 与 FC**：FC 管表达调用、MCP 管标准化接入，二者正交；工具爆炸只看路由。
- **Q06 容错与防死循环**：分解析失败/执行失败；四道防线防跑飞。
- **Q07 记忆/知识库/日志**：会被语义检索并影响下一次决策的才是记忆。
- **Q08 溢出 vs 腐化**：隔离优于压缩；KV Cache 三铁律。
- **Q09 RAG**：混合检索+RRF+重排；Agentic RAG 按需查。
- **Q10 评估**：评测可自动化、优化不能；看 Pass^k；归因找首个错误并分清 Harness/模型。

### 带走三句话

1. Agent 与普通 LLM 的本质区别是有没有自主决策和闭环，循环来自自回归生成机制。
2. 选型：固定用 Workflow、自主决策用单 Agent、有新信息才拆多 Agent；工具侧 MCP 与模型侧 FC 正交。
3. 可靠性：四道防线防跑飞、KV Cache 三铁律省成本、RAG 重在检索准、评估看 Pass^k 并归对层。

### 知识卡

跨全文挖掘 15 张知识卡（概念对比 / 结论 / 数据 / 代码模式 / 易错点），带原文锚点，完整内容见阅读页提炼层。
