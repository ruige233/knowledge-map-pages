---
title: "MoneyPrinterTurbo"
url: "https://github.com/harry0703/MoneyPrinterTurbo"
source: "GitHub / harry0703"
published: "2024-03-11"
archived: "2026-10-02"
category: "生成工具"
tags: [短视频生成, Python, 文本转语音, 字幕, 自动化]
lang: en
status: ok
---

# MoneyPrinterTurbo

> 一句话定位：给一个主题或关键词，自动写出文案、找素材、配音、上字幕、配背景音乐，直接合成一条高清短视频。

## 摘要

MoneyPrinterTurbo 把短视频生产的全流程串成一条自动化流水线：用户只提供主题或关键词，系统依次生成视频文案、抓取或选用素材、合成语音、生成字幕并叠加背景音乐，最后输出成片。项目用 MVC 架构组织代码，同时提供 Web UI 和 API 两套入口，并支持一次批量生成多条视频再挑选。规格上支持竖屏 9:16（1080×1920）与横屏 16:9（1920×1080）两种尺寸，中英双语文案；语音合成可实时试听并支持 Azure 语音；字幕支持 edge 与 whisper 两条路线，字体、位置、颜色、大小、描边均可调；背景音乐可随机或指定文件并单独控制音量。模型接入面很宽，覆盖 OpenAI、Moonshot、Azure、通义千问、Google Gemini、Ollama、DeepSeek、MiniMax、文心一言、ModelScope、gpt4free、one-api、Pollinations 等。硬件门槛不高：4 核 CPU / 4 GB 内存即可跑，GPU 非必需，推荐 8 GB 内存加 4 GB 显存。截至 2026-10-02 快照约 128.0k stars、20.0k forks，MIT 许可，主语言 Python。

## 要点

- **全自动流水线**：文案 → 素材 → 语音 → 字幕 → 背景音乐 → 成片合成，一次触发跑完；支持批量出片择优。
- **规格与硬件**：竖屏 1080×1920 与横屏 1920×1080；最低 4 核 CPU / 4 GB 内存，GPU 非必需，推荐 8 核 / 16 GB 内存 + 8 GB 显存。
- **多模型可换**：文案与关键词环节可接 OpenAI、DeepSeek、通义千问、Gemini、Ollama 等十余家国内外服务，也支持 one-api 聚合与本地 Ollama。
- **字幕双方案**：edge 用于快速生成，whisper 用于更准确的识别；字幕样式可细调字体、位置、颜色、大小与描边。
- **四种部署路径**：Windows 一键启动包、`docker compose up`、uv/venv 手动部署、Google Colab 免本地环境。
- **关注度数据**：2026-10-02 快照约 128.0k stars / 20.0k forks，MIT 许可，创建于 2024-03-11。

## 关联

- 与同分类的 [MarkItDown](MarkItDown.md)、[Open-Generative-AI](Open-Generative-AI.md) 同属内容生成工具；本条目偏"素材拼装成片"，Open Generative AI 偏"模型直接生成画面"。
