---
title: "security-audit"
url: "https://github.com/cloudflare/security-audit-skill"
source: "GitHub / cloudflare"
published: "2026-06-18"
archived: "2026-11-03"
category: "SKILL"
tags: [编码智能体, Agent Skills, Cloudflare, 安全审计, 漏洞挖掘]
lang: en
status: ok
---

# security-audit

> 一句话定位：Cloudflare 出品的编码智能体安全审计 Skill，把智能体编排成多阶段、覆盖账本驱动的漏洞猎手，产出可独立验证、机器可读的审计结论。

## 摘要

security-audit 是 Cloudflare 开源的编码智能体技能包（Agent Skill），把一次代码库安全审计编排成六个阶段：侦察、覆盖账本驱动的狩猎、候选验证、结构化输出、独立记录复核、目标中立的报告生成。它以隔离子代理并行推进，用 `architecture.md` 与 `coverage-ledger.json` 记录架构、信任边界与确定性覆盖，再让"覆盖批评者"找盲区；每个候选漏洞交给从未见过它的验证者去证伪，最终按 `confirmed`（有完整来源链与可观测结果）、`needs_validation`（有未解决事实但不给严重级）、`rejected`（已被证伪）三态写入 `findings.json`，并由零依赖的 `validate-findings.cjs`、`validate-coverage-ledger.cjs` 校验。该 Skill 源自 Cloudflare 博客《Build your own vulnerability harness》所述漏洞发现系统的起点，MIT 许可，通过 `npx skills add` 安装。官方测试中单次运行约只发现重复运行所能发现漏洞的一半。

## 要点

- **六阶段审计流程**：侦察 → 覆盖账本驱动狩猎 → 候选验证 → 结构化输出（`findings.json` 按 `report-schema.json` 校验）→ 独立记录复核 → 生成 `REPORT.md` / `FINDINGS-DETAIL.md` / `NEEDS-VALIDATION.md`。
- **覆盖账本机制**：`coverage-ledger.json` 把审计面切分为确定性单元，隔离的 hunter 各自认领并记录检查，coverage critic 专门找未覆盖的盲区；父代理在创建账本及每次更新后运行 `validate-coverage-ledger.cjs`。
- **对抗式验证**：验证发现的代理永远不是发现它的那个；验证者任务是"证伪"而非"确认"，材料替换后还要再过一次独立验证。
- **三态裁决**：`confirmed` 要求完整来源链 + 有边界的观测结果；`needs_validation` 保留确切的未解决事实且不给严重级；`rejected` 记录被证伪的候选。
- **增量可重复运行**：多次对同一仓库运行是累加式的，复用旧账本与发现，针对缺口继续挖、重新验证变更源，不把过期或未完成的工作当作已覆盖。
- **配套文件**：15 个攻击类提示文件覆盖内存安全/二进制、AI 与 LLM、Web 协议与认证、客户端、供应链与发布、云与部署、RPC 与消息、资源耗尽、数据隔离等场景。
- **硬性要求**：支持工具调用与并行子代理的编码智能体；Node.js 跑校验器；对目标构建/测试/进程必须有 OS 级沙箱（禁外网、白名单环境、资源限制、仅写指定临时目录），否则只把线索保留为 `needs_validation` 而不执行目标代码。
- **设计原则**：只确认已建立的边界失败；严重性 = 可能性 × 影响而非清单偏差；纵深防御缺口不算漏洞（Layer A 已防住时，缺 Layer B 只是加固建议）。

## 关联

- `安全工具/Android-Reverse-Engineering-Skill.md` — 同为安全方向的 Agent Skill：它把 APK 反编译与调用链梳理固化为 Claude Code 流程，本文把通用安全审计固化为多阶段编排。

## 注意

- Stars 为滚动值：本次 GitHub API 快照（2026-11-03）为 57,915 stars / 3,773 forks，随仓库热度变化，勿当作固定常量。
- `published` 取仓库创建日期（2026-06-18），非最后提交时间（`pushed_at` 2026-09-14）。
- GitHub API 返回 `topics` 为空数组，标签按 README 内容自行拟定。
