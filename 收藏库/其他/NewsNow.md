---
title: "NewsNow"
url: "https://github.com/ourongxing/newsnow"
source: "GitHub / ourongxing"
published: "2024-09-23"
archived: "2026-10-02"
category: "其他"
tags: [新闻聚合, Next.js, MCP, 自托管]
lang: en
status: ok
---

# NewsNow

> 一句话定位：一个可自托管的实时热点新闻聚合站，把微博、知乎、GitHub、Hacker News 等中英文信息源汇到无广告的统一阅读界面，并可作为 MCP 数据源接入 AI 工作流。

## 摘要

NewsNow 是用 Next.js 构建的开源实时热点新闻聚合平台，把微博、知乎、GitHub、Hacker News 等中英文信息源汇总到统一的无广告阅读界面。它的关键设计是按各信息源的更新频率自适应调整抓取间隔——默认约 30 分钟、最快约 2 分钟——以减少无效请求并降低 IP 被封的风险，数据缓存在自建实例或 Cloudflare D1 中，不经第三方追踪。站点支持 GitHub OAuth 登录与跨设备同步。可部署到 Cloudflare Pages、Vercel 或 Docker，运行时要求 Node.js 20 以上；另提供 MCP Server，可作为 AI 工作流的实时新闻数据源。MIT 许可，快照时约 2.19 万 star。

## 要点

- **自适应抓取间隔**：按源的实际更新频率动态调度爬虫，默认 30 分钟、最快 2 分钟，兼顾时效性与被封锁风险。
- **自托管与隐私**：数据只存在用户自己的服务器 / Cloudflare D1 上，不依赖第三方 SaaS，避免追踪。
- **部署方式**：Fork 后导入 Cloudflare Pages 或 Vercel、`docker compose up`、或本地 `pnpm dev`；需要 Node.js >= 20。
- **登录与首次运行配置**：GitHub App 回调地址为 `/api/oauth/github`；环境变量需要 `G_CLIENT_ID`、`G_CLIENT_SECRET`、`JWT_SECRET`，首次运行设 `INIT_TABLE=true`，缓存由 `ENABLE_CACHE` 控制。
- **MCP Server**：内置 MCP 支持，可把实时热点数据直接喂给 AI 智能体或自动化流程。
- **元数据**：MIT 许可，主语言 TypeScript，仓库创建于 2024-09-23，最后推送 2026-09-16，约 21,944 star、5,975 fork。

## 注意

- **仓库已改名 / 转移**：源笔记记录开发者为 `ourongxing`，GitHub API 返回的仓库全名为 `newsnext/newsnow`。`url` 按源文件原样保留原地址，API 请求会跟随重定向；如需引用权威地址，可改用新全名。
- 源笔记称技术栈为 Node.js + Next.js，API 返回的主语言为 TypeScript，两者不矛盾（Next.js 项目即 TypeScript）。
- star、fork、推送日期均为 2026-10-02 的 API 快照。
