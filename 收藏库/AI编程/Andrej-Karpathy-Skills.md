---
title: "Andrej Karpathy Skills"
url: "https://github.com/multica-ai/andrej-karpathy-skills"
source: "GitHub / multica-ai"
published: "2026-01-27"
archived: "2026-10-02"
category: "AI编程"
tags: [Claude Code, CLAUDE.md, 提示工程, 编码智能体, 编码指南]
lang: en
status: ok
---

# Andrej Karpathy Skills

> 一句话定位：一份单文件 CLAUDE.md 规则集，把 Andrej Karpathy 对 LLM 编码缺陷的观察固化成四条行为准则，用来约束 Claude Code 等编码智能体的工作方式。

## 摘要

项目把 Andrej Karpathy 对 LLM 写代码的批评转成可直接落地的提示规则：模型常擅自做错误假设、不主动澄清歧义、把简单问题过度抽象，还会改动自己并不理解的代码与注释。对应给出四条原则——编码前先想清楚（不确定就问、有歧义就摆出多种解释、有更简单方案就直说）；简单优先（不做单一用途抽象、不加未被要求的可配置性、200 行能压到 50 行就重写）；外科式变更（只改必要处、不顺手重构未损坏的代码、匹配既有风格，发现无关死代码只提不删，但要清理自己造成的无用导入与变量）；目标驱动执行（把命令式指令转成带验证标准的声明式目标并循环验证）。交付物是一份 CLAUDE.md，可作为 Claude Code 插件安装，也可直接写入项目，另附一份 Cursor 规则文件。

## 要点

- **交付物极简**：核心就是一份 `CLAUDE.md`，安装路径为 Claude Code 插件市场（`/plugin marketplace add` 再 `/plugin install`），或 curl 覆盖/追加到项目里；同一套规则另存为 Cursor 的 `.cursor/rules/karpathy-guidelines.mdc`。
- **四条原则**：编码前思考、简单优先、外科式变更、目标驱动执行，每条都写成可判定的禁止项而非抽象口号。
- **核心洞察**：项目引述 Karpathy 的观点——LLM 在"循环到满足特定目标"上表现异常出色，因此应给它成功标准，而不是逐步命令。
- **效果信号**：只有被请求的改动出现、代码第一次就保持简单、澄清提问发生在实现之前、PR 干净且无附带重构。
- **权衡**：规则整体偏向谨慎而非速度，对拼写修复、明显单行改动这类琐碎任务应自行判断，不必全套照搬；目标是减少非琐碎工作的高成本错误。
- **关注度**：截至 2026-10-02 API 快照 216,450 stars、21,810 forks；仓库创建于 2026-01-27，最后推送 2026-04-20。

## 关联

- `教程/Claude-Code-Best-Practice.md`、`教程/Learn-Claude-Code.md` — 同属 Claude Code 的行为规范与使用实践。
- `AI编程/Claude-Context.md`、`AI编程/Context-Mode.md` — 同批条目，都是从上下文/工具侧增强 Claude Code 的能力，本条从提示规则侧约束行为。

## 注意

- 源笔记「基本信息」的来源 URL 为 `multica-ai/andrej-karpathy-skills`，而「相关资源」与安装命令写的是 `forrestchang/andrej-karpathy-skills`；GitHub API 显示两个地址指向同一仓库，规范名（`full_name`）为 `multica-ai/andrej-karpathy-skills`，本条目 `url` 取前者。
- 源笔记把维护者记为 forrestchang，与仓库当前 owner（multica-ai）不一致，疑为账号改名或仓库转移；未进一步核实。
- API 未返回许可证与主语言字段，故不填；仓库无 topics。
- star/forks/日期为 2026-10-02 抓取的 GitHub API 快照，会随时间变化。
