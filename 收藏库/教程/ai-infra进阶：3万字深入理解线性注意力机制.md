---
title: "AI Infra进阶：3万字深入理解线性注意力机制"
url: "https://mp.weixin.qq.com/s/O0_v8wz4iHxbkDLENy7EPg"
source: "微信公众平台 · 腾讯技术工程"
published: "2026-10-29"
archived: "2026-11-09"
category: "教程"
tags: [模型与训练, 大模型]
lang: zh
status: ok
阅读页: "阅读页/教程/ai-infra进阶：3万字深入理解线性注意力机制.html"
---

# AI Infra进阶：3万字深入理解线性注意力机制

> 一句话定位：线性注意力演进：从累加状态到KDA遗忘门与Chunk并行实现。

## 摘要

本文是《大模型架构演进：从内存墙到百万上下文》系列的一篇，作者 binnnliu（腾讯技术工程），面向 Infra 工程与研究者，系统拆解线性注意力的原理与实现。文章从 Decode 阶段的访存瓶颈出发，沿「普通线性注意力 → DeltaNet → GDN → KDA」的演进主线，逐步加入写门、遗忘门与逐通道遗忘门，讲清状态更新从简单累加走向有界、可训练、可并行门控的动机与代价。随后从 RNN 范式局限、时序/因果并行、训练视角（快权重与 Delta Rule）三个侧面补全理论，并以 GLM-5.3-Flash 与 vLLM 为例详解 KDA 的 Prefix Cache、状态快照、Chunkwise 并行与推测解码回滚等工程实现。全文 3 万字，附 5 个公式推导附录，核心结论是：线性注意力的演进本质是在模型表达力、算法并行度与硬件成本之间做 trade-off。

## 要点

- 瓶颈与三条路：Prefill 计算密集、Decode 访存密集；线性注意力把历史 KV 压进固定状态 S∈ℝ^(dv×dk)，Decode 访存与计算降至 O(1)。
- 演进主线是「加门」：普通线性注意力只累加会记忆饱和；DeltaNet 写残差 β(v−Sk)kᵀ 防状态爆炸；GDN 加标量遗忘门；KDA 升维到逐 key 通道的 Diag(α) 遗忘门。
- KDA 状态更新是仿射变换 S←ST+H，这是 Chunkwise 并行的代数基础；纯累加状态满足交换律丢位置信息，遗忘门兼作隐式位置编码。
- 训练视角：把 S 看作从 key 预测 value 的线性模型，对重建损失做一步 SGD 即得 Delta Rule；L2 Norm 与 L2 正则共同保证状态转移谱范数 ≤1、不放大旧状态。
- 工程实现（GLM-5.3-Flash / vLLM）：UniformTypeKVCacheSpecs 合并异构层 block table；align 模式只存块边界末 token 状态保持 O(1) 显存；Marconi 式 APC 借 Full Attention 命中反推分叉点；推测解码为候选 token 分别落盘状态快照再回滚。
- 总结论：线性注意力、MLA、DSA 等本质都是对全注意力的适度妥协，用表达能力换计算与访存效率，是算法与体系结构的 Hardware-Software Co-design。

## 关联

- 《为什么AI会“选择性失忆”：搞懂Transformer架构》（`收藏库/教程/为什么AI会“选择性失忆”：搞懂Transformer架构.md`）——注意力机制与 KV 记忆的基础，本文是其在「压缩注意力」分支上的进阶。
- 《一文拆解DeepSeek Harness》（`收藏库/教程/一文拆解deepseek-harness.md`）——Harness 与上下文工程视角，可对照理解 vLLM 运行时层如何承接 KDA 这类新架构。
- 《多Agent协作的下一场竞争，不在模型，而在上下文》（`收藏库/AI应用/多agent协作的下一场竞争，不在模型，而在上下文.md`）——同为「上下文」主题，一篇讲协作产品形态，一篇讲底层注意力架构。

## 提炼

> 一句话定位：线性注意力演进：从累加状态到KDA遗忘门与Chunk并行实现。

### 分节提炼

**背景**

- Prefill计算密集、Decode访存密集，瓶颈在历史KV反复读取
- 百万上下文下KV Cache的显存、带宽、计算量均线性增长
- 三条解法：稀疏注意力、压缩注意力、线性注意力
- Continuous Batching与PagedAttention分摊权重读取、减碎片
- 易错：Continuous Batching帮不了Attention层分摊历史KV读取
**从线性注意力到 KDA的演进**

- 线性注意力把历史压进固定状态S∈ℝ^(dv×dk)，Decode O(1)
- 普通线性注意力只累加无遗忘，记忆饱和甚至数值爆炸
- DeltaNet写残差β(v−Sk)kᵀ，转移I−βkkᵀ防状态爆炸
- GDN加标量遗忘门，KDA升维到每key通道的Diag(α)
- 易错：去分母后输出变线性组合，需输出端归一化防梯度不稳
- 易错：遗忘门维度越高，门值开销与并行效率需trade-off
**理解KDA：科学的假设**

- KDA每head维护固定状态S，由q、k、v与α、β、γ三门更新
- 遗忘门S_{t-1}Diag(α)按key通道保留或遗忘历史
- 写门β与Delta修正减掉与k_t匹配的旧内容，精准覆写
- 易错：遗忘、写、输出三门职责不同，勿混淆α、β、γ
**RNN 范式的局限**

- 固定状态必有损压缩，长文本精准召回能力降低
- 门控与Delta只能改善选择，无法给固定状态无限容量
- 折中是Hybrid：线性层聚合历史、Attention保留直访通道
- 易错：保留全注意力层则其KV Cache仍随长度线性增长
**时序/因果并行**

- 经典RNN把非线性嵌入状态反馈链，难以沿时间维并行
- Softmax非线性不建位置间递归，各Query行可并行调度
- KDA状态更新为仿射变换S←ST+H，三阶段Chunkwise并行
- 纯累加状态交换律丢位置，遗忘门兼作隐式位置编码
- 易错：RoPE不能保证距离越近分数越高，超训练范围需验证
- 易错：第一层因果Attention无序，无法区分[A,B,C]与[B,A,C]
**从训练的角度理解演进**

- 从fast-weight视角，S是随上下文更新的动态关联记忆
- DeltaNet以重建损失对快权重做一步SGD得Delta Rule
- L2正则的1−ηλ与GDN遗忘门形式一致，即快权重衰减
- GDN先衰减旧记忆，再用衰减后记忆算误差并纠正
- 易错：不能仅凭加了L2正则就断言任意步长都衰减，需0<ηλ<1
**KDA具体实现**

- vLLM用KVCache Group管理同构层block table，Uniform合并异构层
- align模式只存block_size边界末token的S，显存保持O(1)
- Marconi式APC借Full Attention命中反推分叉点为后来者存S
- 推测解码为候选token分别保存状态快照，按接受数索引回滚
- 易错：mamba_cache_mode=all会无效写HBM中间态，浪费算力带宽
- 易错：prefix-match-unit只解决尾部粒度，中间分叉点仍整块对齐
**总结**

- 演进核心：表达力、代数并行度、硬件成本三者trade-off
- Linear Attention/MLA/DSA等本质是对全注意力的适度妥协
**reference**

- 列出线性注意力、KDA、vLLM、推测解码等论文与资料
- 涵盖位置编码、Prefix Cache与SSM相关参考文献
**附1. chunk内 A和M 的计算**

- A与M衰减区间都是j+1到i，区别在向量、系数与对角线
- 直接一次矩阵乘算A会因e^320超fp32上限而溢出
- 16×16子块用参考点拆指数，非对角矩阵乘、对角逐列算
- 易错：溢出后再加因果掩码也无法修复已溢出的下三角项
**附2: 为什么要L2 Norm**

- L2 Norm让k_tᵀk_t=1，约束I−βkkᵀ谱范数≤1不放大旧状态
- 单位key下沿k_t读出是旧读出与新值的加权平均
- 正交查询读出在DeltaNet下不变，KDA仍受遗忘门作用
- 易错：语义不相关不等于数学正交，正交条件须k_tᵀq=0
**附3：链式法则**

- 损失对s求导：外层平方、内层v−sk，链式法则相乘
- 把外层1/2 x²导数x与内层−k相乘得−(v−sk)k
**附4: 哈希粒度**

- scheduler_block_size取各group block_size的LCM保证对齐
- hash_block_size取prefix-cache group的GCD或prefix_match_unit
- 改prefix_match_unit只改哈希粒度，不动物理block_size
**附5: 为什么开启 EAGLE/MTP 时，Prefix Cache 命中必须无条件回退一个块（use_eagle_block_drop）？**

- Draft KV用到下一token，命中块尾槽位可能被后续token污染
- 调度器不存t[k]与hidden，只能无条件回退一个块/hash单位
- 命中上限num_tokens−1，整倍数长度时最后一个满块也重算
- 易错：只有分叉点恰在块边界才真污染，但调度器无法区分

### 知识卡（精选）

1. 概念对比｜普通线性注意力 vs DeltaNet：普通线性注意力只累加S←S+vkᵀ，同key写两次取两次value之和；DeltaNet先查旧状态再写残差β(v−Sk)kᵀ，避免重复写入与数值爆炸。
2. 概念对比｜GDN vs KDA 遗忘门：GDN每head一个标量α衰减整张状态，所有通道同速遗忘；KDA升维为每key通道的Diag(α)，第j通道拥有专属遗忘速度α_t,j。
3. 结论｜仿射变换是Chunk并行关键：KDA状态更新S_t=S_{t-1}T_t+H_t是仿射变换；T_t、H_t由当前层输入独立准备，无需等递归状态，故可块内预计算、块间按序传。
4. 结论｜遗忘门兼作位置编码：纯累加状态S←S+vkᵀ满足交换律，[A,B]与[B,A]状态一致，丢失位置信息；遗忘门让越远的token衰减越多次，隐式注入距离。
5. 易错点｜去分母需输出归一化：The Devil in Linear Transformer指出分母φ(q)ᵀZ可能极小致梯度不稳；去分母后输出变线性组合，需在输出端做LayerNorm/RMSNorm。
6. 结论｜L2 Norm保证不放大：单位key下I−βkkᵀ在k方向特征值1−β、正交补空间为1，旧状态转移谱范数≤1，只收缩或保留不放大，防止状态爆炸。
7. 数据｜GLM-5.3-Flash KDA状态尺寸：conv_state[3072,3]bf16占18,432B，recurrent_state[8,128,128]fp32占524,288B，合计542,720B；block_size对齐到640。
8. 数据｜MLA vs Indexer 页大小：MLA每token 1024B、一页655,360B；Indexer每token 132B、一页8.25KiB；UniformType合并后一个block约4.78MiB。
9. 代码模式｜KDA核心伪代码：q,k=l2norm(q),l2norm(k)；S←S⊙α遗忘；Δv←v−Sk残差；S←S+(β·Δv)⊗k写入；o←S(q·head_dim^{-1/2})读出；o←RMSNorm(o)⊙γ。
10. 结论｜重建损失导出Delta Rule：把S看作从key预测value的线性模型，以β为学习率对重建损失L=‖v−Sk‖²/2做一步SGD，即得Delta Rule：只写入旧记忆尚未预测对的部分。
11. 易错点｜A矩阵不能一次矩阵乘：衰减因子同时依赖i、j，直接拆e^{Gi}⊙e^{−Gj}一次矩阵乘会因G≈−320时e^320超fp32上限溢出，事后加因果掩码也无法修复。
12. 易错点｜推测解码状态污染：recurrent_state S原地混合所有token，被拒绝的draft token一旦更新进去就不可逆污染历史；必须为候选token分别落盘状态快照后再回滚。
13. 结论｜align模式只存边界状态：默认mamba_cache_mode=align：只保存scheduler step最后token的S，且仅当位于i*block_size才注册前缀缓存，中间状态填null_block。
