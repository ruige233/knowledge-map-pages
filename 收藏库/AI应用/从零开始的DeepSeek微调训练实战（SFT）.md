---
title: "从零开始的DeepSeek微调训练实战（SFT）"
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA%3D%3D&mid=2247547117&idx=1&sn=a81b273ee3004d6931fbc8cdc9b4c912"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI应用"
tags: [AI应用, DeepSeek]
lang: zh
status: ok
阅读页: "阅读页/AI应用/从零开始的DeepSeek微调训练实战（SFT）.html"
---

# 从零开始的DeepSeek微调训练实战（SFT）

> 一句话定位：用unsloth微调DeepSeek R1 Distill 7B医疗CoT实战

## 摘要

本文是DeepSeek R1蒸馏模型的高效微调实战教程。先辨析微调、强化学习与模型蒸馏的异同，再讲全量微调与LoRA/QLoRA的原理及应用场景。基于unsloth框架，约7G显存、半小时可完成最小实验：下载DeepSeek-R1-Distill-Qwen-7B，选用25371条medical-o1-reasoning-SFT医疗CoT数据。实操演示模型加载、问答模板、初始模型测试、数据拼接与SFTTrainer微调、模型合并，并给出LoRA超参解析与全量代码。对比微调前后：初始模型医疗问答不规范甚至答错，微调后更规范。

## 要点

- 用unsloth对DeepSeek R1 Distill 7B做LoRA/QLoRA微调，7G显存、半小时跑通最小实验。
- 微调、强化学习、模型蒸馏目标不同：调参适配、试错学策略、知识压缩迁移。
- LoRA只训练低秩适配器，QLoRA再对适配器量化（INT4/INT8），显存更省。
- DeepSeek R1回复含think与response，微调数据必须两者兼备，否则指令消融。
- 选用medical-o1-reasoning-SFT（25371条医学CoT）做医疗知识灌注与推理提升。
- 数据格式化：按train_prompt_style拼接Question、Complex_CoT、Response并加EOS_TOKEN。
- SFTTrainer关键超参：r=16、lora_alpha=16、lr=2e-4，全量num_train_epochs=3，最小实验max_steps=60。
- 微调前医疗问答不规范甚至答错，微调后更规范，验证微调有效。

## 提炼

> 一句话定位：用unsloth微调DeepSeek R1 Distill 7B医疗CoT实战

### 分节提炼

- **01 基础概念**：微调在预训练模型上用小量标注数据再训练，适应特定任务，计算量较小。；强化学习靠与环境交互试错获得奖励，学习最优策略，用于游戏、机器人等。；模型蒸馏用教师模型软标签训练更小的学生模型，实现压缩与低功耗部署。；LoRA只训练低秩适配器，QLoRA再把适配器量化到INT4/INT8，更省显存。
- **02 环境准备**：unsloth让Llama 3.3/Mistral/Phi-4/Qwen 2.5/Gemma微调提速2倍、省80%内存。；wandb用于训练监控，注册后拷贝API key，pip install wandb安装。；从ModelScope用modelscope download下载DeepSeek-R1-Distill-Qwen-7B到本地。；DeepSeek R1微调数据须含think与response两部分，缺think会指令消融。
- **03 微调实操**：加载模型关键参数：max_seq_length=2048、dtype=None、load_in_4bit=False，显存不足可设True。；LoRA配置：r=16、lora_alpha=16，target_modules选q/k/v/o/gate/up/down_proj。；数据格式化：按train_prompt_style拼接Question、Complex_CoT、Response并追加EOS_TOKEN。；训练：最小实验500条数据max_steps=60；全量num_train_epochs=3、lr=2e-4；保存用save_pretrained_merged。

### 带走三句话

1. 推理模型微调关键是数据：回复必须同时含think与response，否则指令消融。
2. 先用500条数据、max_steps=60做最小实验验证，再决定是否全量训练。
3. LoRA/QLoRA让小显存也能微调7B模型：7G显存、半小时即可跑通。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
