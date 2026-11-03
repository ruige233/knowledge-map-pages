---
title: "caveman"
url: "https://github.com/JuliusBrussee/caveman"
source: "GitHub · JuliusBrussee"
published: "2026-04-04"
archived: "2026-11-03"
category: "AI编程"
tags: [Caveman, Claude Code, Token优化, Agent Skills, 提示工程]
lang: en
status: ok
---

# caveman

> 一句话定位：caveman 是一个让编码智能体「说更少、读更少」的技能 + 本地代理，用「穴居人」风格压缩输出、压缩日志/JSON 等输入，实测省 33% 输入 token、54% 成本，而代码与命令逐字不变。

## 摘要

caveman 是 JuliusBrussee 开源的 token 节省工具（Go 实现，Apache-2.0），由「穴居人」风格 Agent Skill 与本地代理组成。核心思路：让编码智能体回答更短、读得更少，但代码、命令、路径、错误信息保持逐字不变。4.0 拆为六块「石头」：Say less（输出压缩，63→20 token 同解）、Read less（日志/JSON/CSV 等输入压缩，会话省 33.2%）、Auto（按任务难度路由模型与 effort，成本降 54%）、Blocks（代码模式复用脚本）、Waste fixes 与 Browse（网页压缩 129.8×）。一条命令 `npx @caveman-ai/cli@latest` 安装，支持 Claude Code、Codex、Gemini CLI 等 30+ 智能体，也可只装 skill。第三方验证：Adobe Research 成本降 1.4–2.4×，Elastic 响应 token 降 63.6%，JetBrains A/B 无显著质量损失（p=0.82）。截至 2026-11-03 约 11.7 万 star。

## 要点

- **原理边界**：不重写用户 prompt（Adobe 的 CAVEWOMAN 研究表明改用户 prompt 会让回答变差），只压缩智能体「说」和「读」的内容；代码、命令、路径、错误信息逐字保留。
- **Say less 规则**：先给答案（`[thing] [action] [reason]`）、一句一个意思（借鉴航空维修手册受控英语 ASD-STE100）、否定词与数字单位绝不省略、无「让我/我认为」等戏份；有安全警告或用户困惑时恢复正常完整表达。
- **Read less 实测**：文件级压缩 CSV 98.9%、日志 98.5%、YAML 99.1%、测试输出 98.9%；六类文件整体会话输入 token 少 33.2%，18/18 题答案正确。
- **Auto 路由**：在模型选择器里选 Auto，按任务难度自动选模型与 effort；85 个真实编码任务（SWE-Interact/SWE-Together）成本为 Opus 5.5 xhigh 的 0.46×（省 54%），中位耗时 35.2→15.9 分钟。
- **Blocks**：把 agent 反复重写的脚本固化为仓库内复用块（`json-peek`、`test-summary`、`grep-defs` 等 8 个），`go test -v` 输出从 397 token 降到 76 token；有数据表明一台机器一周内 agent 重写同一脚本 394 次。
- **安装与隐私**：`npx @caveman-ai/cli@latest`（Node 22.13+）；CLI 默认发匿名使用统计，`caveman telemetry off` 或 `DO_NOT_TRACK=1` 可关；Auto 需登录并上传最近请求上下文，`caveman off routing` 可停。

## 关联

- 与 `SKILL/Anthropic-Skills.md`、`Agent框架/如何写好 Skill：一份终极实战经验手册.md` 相关：caveman 是 Agent Skills 生态里「一个 skill 解决单一痛点」的代表，也是 prompt 压缩/工程化实践的参考。

## 注意

- star 数为滚动值：2026-11-03 抓取时约 117,100（README 中 shield 徽标会每日刷新），后续变化属正常。
- 仓库 `published` 取创建日期 2026-04-04，最近推送 2026-11-03；官方文档在 https://docs.caveman.so/caveman。
