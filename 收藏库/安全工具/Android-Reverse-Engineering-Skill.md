---
title: "Android Reverse Engineering Skill"
url: "https://github.com/SimoneAvogadro/android-reverse-engineering-skill"
source: "GitHub / SimoneAvogadro"
published: "2026-02-02"
archived: "2026-10-02"
category: "安全工具"
tags: [Android, 逆向工程, Claude Code, Agent Skill, jadx]
lang: en
status: ok
---

# Android Reverse Engineering Skill

> 一句话定位：一个给 Claude Code 用的 Android 逆向技能包，把 APK 反编译、网络接口挖掘和调用链梳理固化成智能体可执行的标准流程。

## 摘要

这是一个面向 Android 应用逆向工程的开源 Agent Skill（Claude Code 技能包），把反编译与代码审计固化成智能体可执行的流程。它支持 APK、XAPK、JAR、AAR 四种输入格式，默认用 jadx 与 Fernflower 双引擎反编译，并自动识别网络层线索：Retrofit 端点、OkHttp 调用、硬编码 URL、认证方式与 Token 位置。它还提供调用链追踪，从 Activities / Fragments 经 ViewModels、Repositories 追到 HTTP 请求层，并能处理 ProGuard 或 R8 混淆代码。项目为 Apache-2.0 许可，运行需 jadx CLI，快照时约 7,950 star。

## 要点

- **输入格式**：APK、XAPK、JAR、AAR 四类，覆盖安装包、拆分包与库文件。
- **双引擎反编译**：jadx 与 Fernflower 并用，互为补充以提升反编译结果可读性。
- **自动线索提取**：识别 Retrofit 端点、OkHttp 调用、硬编码 URL、认证模式与 Token 位置，直接给出接口清单与凭据暴露点。
- **调用链追踪**：Activities / Fragments → ViewModels → Repositories → HTTP 调用层，用于把界面行为映射到底层请求，便于定位鉴权与数据流。
- **混淆对抗**：支持 ProGuard 与 R8 混淆后的代码分析，这是 Android 商业应用逆向的常态前提。
- **前置依赖与元数据**：需 Java JDK 17+ 与 jadx CLI；Apache-2.0 许可，约 7,950 star、904 fork，创建于 2026-02-02，最后推送 2026-09-30。

## 关联

- `安全工具/HackingTool.md`：后者的「逆向工程」「移动安全」分类提供通用工具集，本技能则是针对 Android 网络层审计的专用流程，可组合使用。
- `SKILL/Anthropic-Skills.md`：同属 Claude Code / Agent Skill 形态，可对照技能包的目录与渐进式披露组织方式。

## 注意

- 源笔记称 star「接近 5000」，GitHub API 2026-10-02 快照为 7,950，源数据已过时，以 API 为准。
- 源笔记未提供发布日期，`published` 取仓库创建日期 2026-02-02。
- API 返回的 topics 为空，仓库无主题标签；能力范围（双引擎、调用链层级、混淆支持）取自源笔记，未逐条对照仓库文档核实。
- `url` 按源文件原样保留。
