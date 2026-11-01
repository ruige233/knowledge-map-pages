---
title: "你不知道的 Agent：原理、架构与工程实践"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247559745&idx=1&sn=31903f96e842d95a2fa2f6b5a5a012cc"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent框架, Agent]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/你不知道的 Agent：原理、架构与工程实践.html"
---

# 你不知道的 Agent：原理、架构与工程实践

> 一句话定位：拆解 Agent 稳定落地的关键：Harness、上下文、工具、记忆与安全边界

## 摘要

文章围绕 Agent 工程化影响效果的关键环节展开：控制流、上下文、工具、记忆、多 Agent、评测、追踪与安全，并用 OpenClaw 串起各原则。核心观点：Agent Loop 是感知-决策-行动-反馈的稳定循环，新能力走工具、提示与状态外化接入；决定稳定性的往往不是模型而是 Harness 的验证质量；上下文分层管理对抗 Context Rot；工具按 ACI 面向目标设计；记忆分四层且整合可回退；长任务靠状态外化；多 Agent 先定协议与隔离；评测先修体系再动 Agent。

## 要点

- Agent Loop 抽象后不到 20 行：感知-决策-行动-反馈循环，能力走工具、提示结构、状态外化三条路径叠加而非改造循环体。
- Harness（验收基线、执行边界、反馈信号、回退手段）比模型更决定系统稳定性，高验证任务尤甚。
- 上下文失效主因是 Context Rot：信号被噪声稀释，需按使用频率与稳定性分层管理。
- Prompt Caching 依赖精确前缀匹配，稳定常驻层换来后续读取高达 90% 的折扣。
- 工具按 ACI 原则面向 Agent 目标设计，描述像路由条件、附反例与示例，调试时先查工具描述。
- 记忆分工作/程序/情景/语义四层，MEMORY.md 精选事实注入，整合只移指针不删原始消息、必须可回退。
- 长任务用 Initializer+Coding Agent 双角色，进度写 JSON 文件外化，慢 I/O 放后台线程。
- 多 Agent 先有协议、任务图、隔离边界再谈并行；评测 Pass@k 探能力、Pass^k 保回归，分数异常先查环境与评分器。

## 提炼

> 一句话定位：拆解 Agent 稳定落地的关键：Harness、上下文、工具、记忆与安全边界

### 分节提炼

- **01 Agent Loop 运转方式**：不到20行的循环：输入→模型响应→tool_use则执行工具回填→文本则返回；新能力走工具集、系统提示、状态外化三条路径，叠加在循环外；Workflow路径代码定，Agent由LLM动态决策，区别在控制权归谁；常是提示链、路由、并行、编排器-工作者、评估器-优化器的组合
- **02 Harness 比模型更关键**：Harness四要素：验收基线、执行边界、反馈信号、回退手段；3人5个月百万行代码，靠约束编码化、端到端自主、最小合并阻力；任务按清晰度与验证自动化分四象限，Harness把任务推向可自动判对
- **03 上下文工程决定稳定性**：无关内容占比高会稀释信号致Context Rot，需分层管理上下文；Prompt Caching靠精确前缀匹配，稳定前缀后续读取折扣达90%；Skills延迟加载：提示只留索引，知识触发注入，描述带反例；压缩易丢决策与约束，保留优先级写CLAUDE.md，不改标识符；文件系统做上下文接口，Cursor的MCP描述外置token降46.9%
- **04 工具设计决定能力边界**：5个MCP服务器约55000 tokens定义开销，工具过多稀释注意力；演进：API封装→ACI面向目标→Tool Search/代码编排/示例驱动；好工具粒度对齐目标、错误结构化含建议、描述写何时用与不用；框架内事件与LLM消息隔离，LLM只收user/assistant/tool_result
- **05 记忆系统设计**：四类记忆：工作/程序/情景/语义，分存窗口/Skills/JSONL/MEMORY.md；ChatGPT不用向量库：约33条偏好、约15个摘要、当前会话窗口；OpenClaw用日志+MEMORY.md+混合检索（70%向量+30%关键词）；整合触发tokenUsage/maxTokens≥0.5，失败写archive/，只移指针
- **06 逐步放开自主度**：自主度前提：跨session续跑、进度约束、慢I/O后台接入；Initializer+Coding Agent双角色，进度写文件，崩溃后断点续跑；任务状态写成JSON外部对象，同时只一个in_progress，先更新再继续；慢subprocess放后台线程，经通知队列下轮LLM调用前注入结果
- **07 多 Agent 组织**：统筹者模式异步委派，把人的持续参与变成对工件的最终审核；协议：JSONL inbox带request_id/status，task图与worktree隔离；子Agent独立messages[]只回摘要；设深度上限与最小提示防外泄；幻觉层层放大，交叉验证或外部反馈能打断错误收敛链
- **08 Agent 评测**：三组概念：task/trial/grader、transcript/outcome、两harness；Pass@k探能力上限，Pass^k保回归，不能混用；代码评分器确定性最高；模型评分器判语义；人工评分器校准；20-50个失败案例即可启动；环境干净起步，正反例都覆盖；分数下降先查环境与评分器再动Agent，别基于失真信号改
- **09 追踪执行过程**：Trace记完整Prompt、messages[]、工具调用、推理链、输出与消耗；两层观测：人工标注摸失败模式，LLM自动评估全量并校准；在线评测采10%-20%Trace，负反馈100%进队列，变更后48h全量；事件流在tool_start/tool_end/turn_end发事件，一次发布多路消费
- **10 OpenClaw 落地实践**：五层解耦：Gateway、Channel适配器、Pi Agent、工具集、上下文记忆；MessageBus隔离渠道与AgentLoop；不同session并发，同session串行；提示分层：SOUL/AGENTS/TOOLS/USER/MEMORY/Skills索引+运行时注入；cron按计划触发、heartbeat每5分钟轮询；任务进度写盘可恢复；安全先于功能：白名单、路径检查、execFile防注入、审计日志
- **11 常见反模式**：八个反模式：提示当知识库、工具失控、缺验证、多Agent无边界；修法：知识移Skills、合并工具、验收标准、隔离、整合、建测试
- **12 划重点**：十条结论覆盖循环、Harness、上下文、工具、记忆、状态、协议；跑稳靠消息解耦、状态外化、分层提示、记忆整合与安全边界

### 带走三句话

1. Harness 与验证质量比模型本身更决定 Agent 能否稳定收敛
2. 上下文要分层稳定，工具面向目标、写清边界并附反例，调试先查定义
3. 状态外化到文件、协议隔离先行、先修评测再改 Agent，比堆模型有效

### 知识卡

跨全文挖掘 14 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
