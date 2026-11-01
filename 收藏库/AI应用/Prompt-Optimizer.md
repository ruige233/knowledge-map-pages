---
title: "Prompt Optimizer"
url: "https://github.com/linshenkx/prompt-optimizer"
source: "GitHub / linshenkx"
published: "2025-02-12"
archived: "2026-10-02"
category: "AI应用"
tags: [提示工程, 提示词优化, 多模型, MCP, 自我托管]
lang: en
status: ok
---

# Prompt Optimizer

> 一句话定位：开源提示词优化工具，把「写好提示词」做成可迭代、可评估的流水线，并提供网页、桌面、Chrome 扩展与 Docker 四种使用形态。

## 摘要

Prompt Optimizer 由 linshenkx 开发，定位是提示词的优化、测试、评估与收藏环节：提示词可以来自手写、模板、本地导入或 Prompt Garden，经它优化后存为可复用的提示词资产。核心能力包括一键优化与多轮迭代、系统提示词与用户提示词双向优化、以及分析／单结果评估／多结果对比评估，用来判断提示词是否真的变好。它兼容 OpenAI、Gemini、DeepSeek、Grok、智谱、硅基流动、MiniMax 等主流模型，并支持文生图、图生图与多图生成，还可经 MCP 接入 Claude Desktop。架构上强调纯客户端处理，数据只在浏览器与你配置的服务商之间直连，不经中间服务器，另可设访问密码。截至 2026-10-02 快照约 36.1k stars、4.2k forks，AGPL-3.0 许可。

## 要点

- **纯前端架构的代价**：所有 Web 形态（在线版、Vercel、Docker）都是纯前端应用，受浏览器 CORS 与混合内容策略限制——连本地 Ollama 需设 `OLLAMA_ORIGINS=*`，HTTPS 页面无法直连 HTTP 本地服务，官方推荐改用桌面版绕过。
- **桌面应用的三点优势**：无 CORS 限制、安装版可自动更新、不依赖浏览器；macOS 侧未做 Apple 签名，若提示「已损坏」需用 `xattr -rd com.apple.quarantine` 去除隔离属性。
- **MCP 集成**：Docker 部署时 MCP Server 随应用启动，走同端口的 `/mcp` 路径，暴露 `optimize-user-prompt`、`optimize-system-prompt`、`iterate-prompt` 三个工具，可用 `MCP_DEFAULT_MODEL_PROVIDER` 指定默认模型。
- **安全提醒（README 明确）**：不要在前端部署中预置 API Key，因为 `VITE_*` 变量会暴露在浏览器产物中；私有部署可用 `ACCESS_PASSWORD` 加访问控制。
- **评估闭环**：除优化外还提供分析、单结果评估、多结果对比评估与「评估驱动的智能重写」，用于验证提示词改动是否真的有效。
- **许可与规模**：AGPL-3.0——可自由使用、修改与商用，但对外提供网站或服务必须开源；仓库创建于 2025-02-12，默认分支为 `develop`。

## 关联

- 与 `AI应用/Prompts.chat.md` 相关：同属提示词资产工具链——Prompts.chat 侧重提示词的来源与共享，Prompt Optimizer 侧重优化、测试与收藏，且 README 明确支持从 Prompt Garden 导入提示词。
- 与 `AI应用/NoteGen.md` 相关：两者都以 MCP 方式把自身能力接入 AI 客户端，但 NoteGen 偏个人知识管理，Prompt Optimizer 偏提示词优化与评估。

## 注意

- GitHub 仓库页面本身 `web_fetch` 只返回导航外壳；正文取自仓库默认分支（`develop`）的 `README.md`——`main` 分支下该文件请求返回 404。星标、forks、日期等元数据来自 GitHub API，为抓取当日快照。
- API 返回许可证为 NOASSERTION，而 README 与徽章均标注 AGPL-3.0，本条目采用 AGPL-3.0，此项差异未独立核验。
- `published` 取仓库创建日期 2025-02-12，仓库无单独发布日期；`lang` 记为 en——README 为中英双语（`README.md` / `README.zh-CN.md`），本条目参考英文版。
- 功能清单、路线图（工作区／项目管理待办）与各部署方式说明均来自项目自述，未实际部署核验。
