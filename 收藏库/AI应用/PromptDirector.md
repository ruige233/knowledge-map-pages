---
title: "PromptDirector"
url: "https://github.com/wchao6891/PromptDirector"
source: "GitHub / wchao6891"
published: "2026-07-19"
archived: "2026-10-25"
category: "AI应用"
tags: [Chrome扩展, 提示词管理, MCP, 本地优先, 创意参考库]
lang: en
status: ok
---

# PromptDirector

> 一句话定位：开源、本地优先的 Chrome/Edge 扩展，一键收藏图片、视频与公开提示词为「案例」，以图墙形式管理，并可经本地 MCP 把素材库接入 Claude Code、Codex 等智能体。

## 摘要

PromptDirector（提示词导演）是一个免费开源的浏览器扩展，核心是把散落在 Jimeng、Higgsfield、Midjourney、X、微信、飞书等 20+ 平台上的作品、视频和公开提示词一键收进本地素材库，按「案例」（case）组织成以图为主、带标签和项目的案例墙。除采集与浏览外，它内置 Composer：读取选中案例后提炼可复用 Skill、生成分镜式提示词；内置 Assistant 可在侧边栏抓取当前页面或视频、在库中检索参考并打开案例。素材库默认只存浏览器本地、无需账号，AI 分析为可选且使用用户自己的密钥。库可通过本地 MCP 连接 Codex、Claude Code、WorkBuddy，让智能体检索提示词、读取原图并写回素材。官方另提供 2,000+ 精选案例库（promptdirector.cn）。仓库采用 Apache-2.0 许可，JavaScript 编写，截至 2026-10-25 约 215 stars、44 forks。

## 要点

- **采集覆盖广**：支持 Jimeng、Higgsfield、LiblibAI、Krea、Midjourney、HeyBox、飞书、X/微博/小红书/Reddit、抖音/B 站/YouTube、Pinterest/Behance 等 20+ 站点；对分节编号的文章可自动拆成多个可选案例，多图作品与飞书表格会保留原有分组与布局。
- **Agent 集成走本地 MCP**：设置 → Agent connection → 复制连接请求，粘贴给 Claude Code、Codex 或 WorkBuddy 即可安装并验证连接器；纯云端工作区无法使用这一本地链路，其他 MCP 客户端可用通用配置。
- **Composer 提炼可复用 Skill**：读取选中案例后，可生成可编辑提示词、可复用的创意 Skill 与标签建议；v1.28 起 Skill 保留完整文件与支持材料，可浏览、编辑并整体导出。
- **本地优先与隐私**：案例、媒体、标签默认只存在浏览器，无账号、无广告、无使用分析；AI 功能可选，仅发送用户选定或明确要求的原图给模型；支持备份恢复、跨设备加密同步。
- **可维护性设计**：回收站、重复案例检测（同图/同视频）、自动补全文本标签与库索引，以及批量标签/批量分析。
- **版本与许可**：README 提及最新为 v1.28（完整 Skill）与 v1.27（侧边栏 Assistant）；代码 Apache-2.0，但代码许可不覆盖第三方案例媒体，复用前需核对各案例的出处与权利。

## 关联

- 与 `AI应用/Prompts.chat.md` 相关：同属「提示词资产」工具——Prompts.chat 偏提示词库与共享，PromptDirector 偏视觉参考 + 提示词的采集、管理与复用。
- 与 `AI应用/Prompt-Optimizer.md` 相关：二者都提供 Chrome 扩展形态并支持 MCP 接入，但 PromptDirector 侧重素材收藏与案例墙，Prompt Optimizer 侧重提示词优化评估。
- 与 `Agent框架/OpenAI-Agents-SDK.md` 等 MCP/Agent 条目相关：PromptDirector 通过本地 MCP 把素材库能力暴露给编码智能体。
- 同项目 fork：@ruige233/PromptDirector（https://github.com/ruige233/PromptDirector）与上游 main 完全一致（compare identical、0 ahead / 0 behind），不单列条目；若其后续出现独立提交再单独收录。

## 注意

- 星标、forks、创建/推送时间等元数据来自 GitHub API（2026-10-25 快照：215 stars、44 forks、创建于 2026-07-19、最后推送 2026-10-22）；`published` 取仓库创建日期。
- @ruige233 的同名 fork（创建于 2026-10-25）已于 2026-10-27 经 compare API 核验与上游 main 完全一致（0 ahead / 0 behind，0 stars / 0 forks），故合并进本条、不单独收录。
- README 为英文版（另有 `README.zh-CN.md`），功能与版本号均来自项目自述，未实际安装核验。
- 采集依赖页面可访问且提示词公开；登录态、懒加载与站点改版可能影响结果，官方建议保存前先预览。
