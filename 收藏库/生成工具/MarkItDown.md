---
title: "MarkItDown"
url: "https://github.com/microsoft/markitdown"
source: "GitHub / microsoft"
published: "2024-11-13"
archived: "2026-10-02"
category: "生成工具"
tags: [Markdown, 文档转换, Python, LLM, OCR]
lang: en
status: ok
---

# MarkItDown

> 一句话定位：微软开源的 Python 库，把 PDF、Office、图片、音频等几十种格式统一转成 Markdown，专门喂给 LLM 和文本分析管道。

## 摘要

MarkItDown 解决的是"把异构文档变成 LLM 能读的纯文本"这一前置问题。它定位与 textract 类似，但目标格式只锁定 Markdown，因此会刻意保留标题层级、列表、表格、链接等结构信息，比直接抽文本更利于后续检索与提示词拼装。输入覆盖面很宽：PDF、Word、PowerPoint、Excel、图片（读 EXIF 并可选 OCR）、音频（读元数据并可做语音转录）、HTML、CSV/JSON/XML、ZIP 压缩包、EPub 乃至 YouTube 链接。工程上要求 Python 3.10+，可选依赖按格式拆分安装，官方提供 Docker 镜像，也支持插件机制（如第三方 OCR 插件）以及接 Azure Document Intelligence 做更强的文档解析。截至 2026-10-02 快照，仓库约 188.0k stars、13.9k forks，MIT 许可，主语言 Python，仍在高频维护。典型用途是文档索引、LLM 训练语料准备与文档问答的前处理。

## 要点

- **结构优先的转换策略**：输出 Markdown 而非裸文本，保留标题、列表、表格、链接，是它区别于通用文本抽取工具的核心取舍。
- **格式覆盖**：PDF / Word / PowerPoint / Excel / 图片（EXIF + OCR）/ 音频（元数据 + 转录）/ HTML / CSV / JSON / XML / ZIP / EPub / YouTube URL。
- **两种调用方式**：命令行 `markitdown file.pdf -o file.md`，或 Python API `MarkItDown().convert(path).text_content`。
- **可选依赖 + 插件体系**：`pip install 'markitdown[all]'` 一次装全，也可按格式装；支持第三方插件扩展，并可接入 Azure Document Intelligence 处理复杂版式。
- **环境要求**：Python 3.10+，提供 Docker 支持；官方建议用虚拟环境隔离依赖。
- **关注度数据**：2026-10-02 快照约 188.0k stars / 13.9k forks，MIT 许可，创建于 2024-11-13，最近推送 2026-10-02。

## 关联

- 与同分类的 [MoneyPrinterTurbo](MoneyPrinterTurbo.md)、[Open-Generative-AI](Open-Generative-AI.md) 同属内容生成/处理工具链，但本条目偏文档解析与格式转换。
- 可作为 Agent 文档问答链路的前置解析组件，与 Agent 框架类条目配合使用。
