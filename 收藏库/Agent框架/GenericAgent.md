---
title: "GenericAgent"
url: "https://github.com/lsdefine/GenericAgent"
source: "GitHub / lsdefine"
published: "2026-01-16"
archived: "2026-10-02"
category: "Agent框架"
tags: [计算机控制, 自我进化, 技能树, 分层记忆, TMWebdriver]
lang: en
status: ok
---

# GenericAgent

> 一句话定位：一个用约 3K 行种子代码和 9 个原子工具就让 LLM 接管整台电脑的极简 Agent 框架，核心主张是「不预加载技能，让技能自己进化出来」。

## 摘要

GenericAgent 用约 3000 行核心代码和约 100 行 Agent Loop，配合 9 个原子工具（代码执行、文件读写改、网页感知与 JS 执行、人机确认及两个记忆工具）控制本地电脑，覆盖终端、文件系统、键鼠、屏幕视觉与手机 ADB。设计哲学是「不预加载技能，让技能自己进化」：每解决一个新任务就把执行路径固化为可复用 Skill 写入记忆层，下次同类任务一句话直调。记忆分 L0 元规则到 L4 会话归档五层，上下文窗口压在 30K Token 内，远低于常见 Agent 的 200K–1M。浏览器由 TMWebdriver（本地 WebSocket 服务 + Chrome 扩展）注入真实浏览器以保留登录态，作者称 SannySoft 56/56、bot.incolumitas 36/36 等机器人检测全部通过。MIT 许可，API 快照 14,275 stars、1,664 forks。

## 要点

- **极简体量**：约 3K 行种子代码 + 约 100 行 Agent Loop + 9 个原子工具；核心只依赖 `requests` 等少数轻量包，无需 Playwright、LangChain 或浏览器二进制，能力扩展靠 `code_run` 在运行时自举。
- **五层记忆**：L0 元规则、L1 记忆索引、L2 全局事实、L3 任务 Skills/SOPs、L4 会话归档；官方对比称 <30K 的上下文窗口只是其他 Agent（200K–1M）的零头，噪声更少、成本更低。
- **自我进化闭环**：遇到新任务先自主装依赖、写脚本、调试验证，再把执行路径固化为 Skill 写入记忆层；同类任务之后一句话即可复用（如"监控股票并提醒我"从首次搭流程变成一句话启动）。
- **真实浏览器**：TMWebdriver 由本地 WebSocket 服务加 Chrome 扩展组成，注入持久化 Chrome 会话以保留 Cookie、登录态与正常指纹；README 列出 SannySoft 56/56、bot.incolumitas 36/36、BrowserScan 正常、FingerprintJS 通过、reCAPTCHA v3 得 0.9 真人分。
- **模型与前端**：支持 Claude / Gemini / Kimi / MiniMax 等主流模型，跨平台运行；提供 TUI（tui_v3.py）、Streamlit UI、Windows 桌面端，以及 Telegram、Discord、微信、QQ、飞书、企微、钉钉等 IM 前端。
- **环境注意**：官方要求 Python 3.11 或 3.12，明确不兼容 3.14（与 pywebview 冲突）；Windows 下 TUI 建议改用 Git Bash。

## 关联

- `Agent框架/OpenAI-Agents-SDK.md` — 同属 Agent 框架：OpenAI Agents SDK 侧重多 Agent 编排与任务交接，GenericAgent 侧重单机全系统控制与自我进化。
- `SKILL/Anthropic-Skills.md` — 两者都以 Skill 作为能力沉淀单元，Anthropic 定义 SKILL.md 规范，GenericAgent 则由 Agent 在运行中自动生成 Skill。

## 注意

- 源笔记记录「Star 数 7000+」，与 GitHub API 当日快照 14,275 差距较大，说明源笔记数据陈旧；本条目采用 API 快照（2026-10-02）。
- 机器人检测通过率、reCAPTCHA v3 得 0.9 等数字均来自仓库 README 自述，未独立复现。
- 仓库 README 为中英双语，`lang` 按英文主节记为 `en`。
