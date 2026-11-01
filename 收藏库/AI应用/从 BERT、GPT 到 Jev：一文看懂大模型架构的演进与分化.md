---
title: "从 BERT、GPT 到 Jev：一文看懂大模型架构的演进与分化"
url: "https://mp.weixin.qq.com/s?__biz=MjM5ODYwMjI2MA==&mid=2649804707&idx=1&sn=9291fb648a66fa13fa295fc5a2156b84"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI应用"
tags: [AI应用, 大模型]
lang: zh
status: ok
阅读页: "阅读页/AI应用/从 BERT、GPT 到 Jev：一文看懂大模型架构的演进与分化.html"
---

# 从 BERT、GPT 到 Jev：一文看懂大模型架构的演进与分化

> 一句话定位：Transformer 到 Jev 的架构演进：Jev 不做生成、只出概率

## 摘要

文章从 2017 年 Transformer 讲起，梳理大模型架构沿三条路线分化的过程：Encoder-only（BERT）做填空、Encoder-Decoder（T5）做补全、Decoder-only（GPT）做续写，并解释后者凭训练信号密集、任务形态统一、KV cache 可复用而成为主流。随后介绍 ChatGPT 与推理模型的后训练变化，以及 DeepSeek、Qwen 在 MoE 与 KV cache 压缩上的组件创新。最后聚焦 2026 年 TypeSafe AI 发布的 Jev：它不做文本生成，只对输入材料返回选择、评分、判断三类问题的概率，并以 RLCD 把概率校准作为训练目标；通过与开源 Laya 对照，说明去掉生成不难，难在底层网络的理解能力。

## 要点

- Transformer 用注意力取代循环结构，所有位置可并行计算，是后来扩展到千亿参数的工程前提。
- 2018 年前后分化出三条路线：BERT（Encoder-only 填空）、T5（Encoder-Decoder 补全）、GPT（Decoder-only 续写）。
- Decoder-only 因训练信号密集、任务形态统一、KV cache 可复用而成为主流；ChatGPT 与推理模型只改训练方式、未改架构。
- DeepSeek 与 Qwen 的架构创新集中在 MoE 与 KV cache 压缩，骨干仍是自回归逐 token 生成的 Transformer。
- Jev 不生成文本，只对输入材料返回 Choice/Score/Noul 三类判断的概率，百毫秒级完成。
- RLCD 与 RLHF、RLVR 并列，把概率校准作为训练目标，解决 RLHF 模型过度自信、无法量化不确定性的问题。
- Laya 用 4 亿参数双向 encoder（ModernBERT-large）实现相近输出方式，说明去掉生成不难，难在底层网络理解能力。

## 提炼

> 一句话定位：Transformer 到 Jev 的架构演进：Jev 不做生成、只出概率

### 分节提炼

- **01 Transformer 与机器翻译**：原始结构两段式：Encoder 输出上下文向量，Decoder 经 cross-attention 逐词生成。；贡献是用注意力取代循环：所有位置并行计算，才能堆叠数十层、扩展千亿参数。；Encoder 注意力双向，Decoder 因果；处理单位是分词器切出的 token。
- **02 三条路线分化**：三条路线层结构几乎相同，实质差异只有注意力可见范围与预训练任务。；BERT 填空、T5 补全、GPT 续写；Encoder-only 退居高吞吐判别场景，靠预训练加微调。；Encoder-only 一次前向、毫秒级延迟，适用于检索、推荐、内容审核。
- **03 Decoder-only 主流**：prefill 一次性并行计算 prompt 并缓存键值；decode 逐 token 生成，串行且访存密集。；输出读输入靠同一序列内的自注意力而非 cross-attention。；成为主流三原因：训练信号密集、任务形态统一、KV cache 可复用。；2022 年系统对比：无监督预训练下 Decoder-only 零样本泛化最强。
- **04 后训练与推理模型**：ChatGPT 对话能力来自后训练而非结构变化；o1、DeepSeek-R1 用强化学习生成思考过程。；RLHF 训练奖励模型拟合人类偏好，使输出向高奖励偏移。；推理模型思考仍是逐 token 自回归生成，延迟从秒级升到分钟级。
- **05 前沿模型与主线**：DeepSeek-V4.1-Flash 与 Qwen3.8-27B 仍是自回归逐 token 生成的 Transformer。；创新集中两处：MoE 拆分前馈网络；KV cache 压缩，DeepSeek 用潜向量加稀疏选择，Qwen 用线性注意力。；判断路线看两点：注意力是否因果掩码、结果是否逐 token 生成。
- **06 Jev**：每次请求给 state 与问题，返回 Choice/Score/Noul 三类原语的概率与 confidence。；RLCD 优化校准的决策，与 RLHF、RLVR 并列，定位为快思考 System One。；与 LLM 同源但不进 decode，直接读出概率；与 BERT 分类器的差别在任务由请求指定。；能理解、不能生成：不能写、不能思考、不能当 agent，换来百毫秒延迟与可用概率。
- **07 对照 Laya**：Laya 输出方式与对 Jev 的推断相近，底层是 4 亿参数双向 encoder（ModernBERT-large）。；把选项写进输入、在 [MASK] 位读出分数并 softmax，本质是 BERT 分类器的进化。；零样本表现远不及 Jev，最好成绩靠微调；差距在底层网络的理解能力。
- **08 全貌**：主流模型骨架没变：ChatGPT/推理模型改训练方式，DeepSeek/Qwen 改内部组件。；Jev 保留读取与理解、去掉生成，是同一底层网络在另一类任务上的另一种用法。；适用与否取决于需要自由文本生成还是在有限选项间判断。

### 带走三句话

1. 主流大模型的 Transformer 骨架没变，变化发生在预训练任务、后训练方式与内部组件上。
2. 判断模型属于哪条路线，看注意力是否因果掩码、结果是否逐 token 生成。
3. Jev 不生成文本，只返回判断概率并以校准为训练目标，适合有限选项的无人值守判断。

### 知识卡

跨全文挖掘 11 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
