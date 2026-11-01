---
title: "Thunderbolt"
url: "https://github.com/thunderbird/thunderbolt"
source: "GitHub / thunderbird"
published: "2025-07-23"
archived: "2026-10-02"
category: "聊天工具"
tags: [Tauri, Ollama, 私有化部署, MCP, 端到端加密]
lang: en
status: ok
---

# Thunderbolt

> 一句话定位：Thunderbird 团队做的开源 AI 聊天客户端，自己选模型、自己掌握数据，避免被单一厂商锁定。

## 摘要

Thunderbolt 是 Mozilla 旗下 Thunderbird 团队推出的开源 AI 聊天客户端，核心主张写在仓库描述里：自己选模型、自己拥有数据、消除厂商锁定。它用 Tauri 构建，因此同一套代码能覆盖 Web、macOS、Linux、Windows、Android、iOS 六端，既能接前沿云端模型，也能接 Ollama 跑本地模型；若选择私有化部署，对话数据可以完全不出内网。部署方式上提供 Docker Compose 与 Kubernetes 两种方案，并保持每天发布 Nightly 版本的节奏，说明项目仍处快速演进阶段。企业级能力正在补齐：OIDC 认证、端到端加密、跨设备云同步、Google 与 Microsoft 账号集成，以及对 MCP（Model Context Protocol）的支持。需要注意的是，来源笔记称其"支持"这些企业功能，但从仓库当前状态看更接近开发中/规划中的路线图项，实际可用性应以官方文档为准。截至 2026-10-02 快照约 4.8k stars、329 forks，MPL-2.0 许可，主语言 TypeScript。

## 要点

- **六端一致**：基于 Tauri，覆盖 Web、macOS、Linux、Windows、Android、iOS。
- **模型与数据可控**：云端前沿模型与 Ollama 本地模型二选一，支持私有化部署以阻断数据外泄。
- **部署形态**：Docker Compose 用于单机/小规模，Kubernetes 用于集群化；每日发布 Nightly 构建。
- **路线图能力**：OIDC 认证、端到端加密、跨设备云同步、Google/Microsoft 集成、MCP 支持 —— 属开发中项目，采用前需核对官方文档。
- **许可与关注度**：MPL-2.0，2026-10-02 快照约 4.8k stars / 329 forks，仓库创建于 2025-07-23。

## 关联

- MCP 是 Agent 工具调用的通用协议，可与收藏库 Agent 框架类条目（如 Agent 框架/OpenAI-Agents-SDK 等）对照阅读。
- 与同分类条目同属对话客户端方向，差异在于本条目强调自托管与跨端覆盖。

## 注意

- 来源笔记列出 OIDC、端到端加密、云同步、Google/Microsoft 集成、MCP 为"正在开发"的功能，未给出完成度与版本号，落地状态未经核验。
- 来源笔记标注 star 数为 4000+，与 2026-10-02 API 快照（4772）存在小幅差异，属正常增长。
- 来源笔记未标注许可证与主语言，本条的 MPL-2.0 与 TypeScript 取自 GitHub API。
