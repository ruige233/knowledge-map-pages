---
title: "AI Infra入门干货总结：大模型是如何高效推理的"
url: "https://mp.weixin.qq.com/s?__biz=MjM5ODYwMjI2MA==&mid=2649801625&idx=1&sn=7c7a7d2148c6907f65c9f747717b3197"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI应用"
tags: [AI应用, 大模型]
lang: zh
status: ok
阅读页: "阅读页/AI应用/AI Infra入门干货总结：大模型是如何高效推理的.html"
---

# AI Infra入门干货总结：大模型是如何高效推理的

> 一句话定位：以Llama 3为例，逐环节拆解vLLM推理流程与张量维度变化

## 摘要

本文以 Llama 3 为例拆解 vLLM 推理全流程，逐环节标注维度变化。核心机制：Continuous Batching 把调度下沉到 token 级，多请求复用权重摊薄显存开销；Paged Attention 用 block_table 按块管理 KV Cache 消除碎片。按 Tokenize、Embedding、Transformer Block、LM Head、Sampling 展开，对比 Prefill 计算密集与 Decode 访存密集，FlashAttention 打破内存墙。

## 要点

- Continuous Batching 将调度从请求级下沉到 token 级，让多请求复用权重摊薄显存开销。
- Paged Attention 用 block_table 按块管理 KV Cache，消除显存碎片，支撑高性能推理。
- 推理流程按 Tokenize、Embedding、Transformer Block、LM Head、Sampling 展开，逐环节标注张量维度。
- Prefill 是计算密集，Decode 是访存密集，根源在于权重与 KV Cache 的访存摊销差异。
- FlashAttention 通过 Kernel 融合、分块计算与 Online Softmax 打破内存墙。
- Llama-3-8B 中 MLP 参数与 FLOPs 均约占 70%，是计算大头，Attention 是访存带宽大头。
- v1 调度只保留 RECOMPUTE 抢占：PCIe 搬运 KV 常比重算 prefill 更贵。

## 提炼

> 一句话定位：以Llama 3为例，逐环节拆解vLLM推理流程与张量维度变化

### 分节提炼

- **01 连续批处理**：调度不区分 Prefill/Decode，让已算 token 数追上总 token 数。；四项硬约束：并发请求数、token 预算、模型最大长度、空闲 KV blocks。；按 token 级调度，输入是 Flattened 的 input_ids，消除 Padding。
- **02 Paged Attention**：KV Cache 按块分配，block_table 记录物理块 ID，解决显存碎片问题。；slot_mapping 与 block_table 不带层维度，各层 KV 存同一物理块相同位置。；用 block_size=16 保证块内连续，以少量带宽折损换取吞吐量跃升。
- **03 推理流程**：Tokenize 用 BPE；Embedding 查表把 token ID 变成特征矩阵。；QKV 按列拼接成宽 GEMM；RoPE 用 cos_sin_cache 免算 sin/cos。；FlashAttention 用融合、分块与 Online Softmax 避免矩阵读写 HBM。；Prefill 计算密集，Decode 访存密集；Attention 无法跨请求摊薄访存。
- **04 总结**：理解推理流程的关键是跟踪 shape 变化，经典推理原理并不高深。；内存墙催生 FlashAttention、MLA/DSA；MoE 是把 FFN 拆成专家按需激活。；chunked prefill、Flash-Decoding、SP 解决超长 prompt 问题。；TP 的列切分与行切分可借助本文 shape 变化理解。
- **05 模型配置参数**：hidden_size=4096，32 个 Q 头、8 个 KV 头，GQA 用于压缩缓存。；Fused QKV 宽 6144，FFN 宽 14336，词表 128256。
- **06 运行时变量**：prompt=100，num_tokens=105，decode 算 1 个 token。；seq_lens 为 KV Cache 总长度，等于历史 token 数加新 token。
- **07 权重组成**：MLP 参数占比约 70%、FLOPs 约 70%，是绝对的计算大头。；Attention 参数占比仅约 16%，却是读取动态 KV Cache 的访存带宽大头。；Embedding 与 LM Head 各约 6.5%，维度一致但独立不共享。
- **08 调度详细流程**：内存不足抢占最低优先级 RUNNING 请求，释放 KV Cache 重置进度。；v1 砍掉 SWAP 只留 RECOMPUTE：PCIe 搬运常比重算 prefill 更贵。；抢占后跳过 WAITING 调度，避免内存压力形成恶性循环。
- **09 参考**：列出 FlashAttention 系列、PagedAttention 等推理论文。；另含 GPU 计算、Roofline、KV Cache 与 vLLM 相关参考。

### 带走三句话

1. 理解推理关键是跟 shape 变化走：token ID → hidden_size → logits。
2. Continuous Batching 下沉 token 级；Paged Attention 按块管 KV Cache。
3. Prefill 计算密集、Decode 访存密集；FlashAttention 用 Online Softmax 破墙。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
