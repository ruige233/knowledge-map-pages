---
title: "Agent、MCP、Skill、Cli、Model 一次讲透：老系统接入 AI 的概念地图与三条路径"
url: "https://mp.weixin.qq.com/s?__biz=MjM5ODYwMjI2MA==&mid=2649804252&idx=1&sn=073e0806e5a950fae374c9e5f7a2a8b6"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "Agent框架"
tags: [Agent框架, Agent, MCP, Skill]
lang: zh
status: ok
阅读页: "阅读页/Agent框架/Agent、MCP、Skill、Cli、Model 一次讲透：老系统接入 AI 的概念地图与三条路径.html"
---

# Agent、MCP、Skill、Cli、Model 一次讲透：老系统接入 AI 的概念地图与三条路径

> 一句话定位：一张概念地图讲清五概念关系与老系统接入 AI 的三条路径

## 摘要

文章回答两个问题：Agent、Skill、MCP、CLI、Model 五个概念的关系，以及老系统接入 AI 的切入点。作者用团队打比方：Model 是大脑、Agent 是数字员工、Skill 是岗位手册、MCP 是万能转接头、CLI 是对讲机，并指出 MCP 与 CLI 可嵌套。一次工具调用是一场接力：用户→Agent→Model 规划→Skill→MCP Server→老系统 API→原路返回。老系统改造先体检四维度（API、文档、鉴权、变更风险），再走三条路径：API 封装首选、CLI 包装次选、RPA 兜底。MCP 实现形式分四类：官方 stdio 与 Streamable HTTP、历史 SSE、社区扩展。最后以请假销假（BS）和 PLM（CS）两个案例验证路径选择。

## 要点

- 五概念关系：Model 是大脑、Agent 是数字员工、Skill 是岗位手册、MCP 是万能转接头、CLI 是对讲机
- MCP 与 CLI 可嵌套，MCP Server 内部常封装 CLI，对 Agent 只暴露标准外壳
- 一次调用接力：用户→Agent→Model 规划→Skill→MCP Server→老系统 API→逐层返回
- 老系统改造先体检四维度，再走三条路径：API 封装、CLI 包装、RPA 兜底
- 三条路径优先级：API 封装首选、CLI 包装次选、RPA 兜底且脆弱性递增
- 鉴权安全一案一议，Agent 代操作的身份传递、权限最小化、审计需单独评审
- MCP 官方传输仅 stdio 与 Streamable HTTP，SSE 已被取代只做存量迁移
- 选型：本地 stdio、企业共享 Streamable HTTP 加 OAuth、实时与浏览器场景用 WebSocket

## 提炼

> 一句话定位：一张概念地图讲清五概念关系与老系统接入 AI 的三条路径

### 分节提炼

- **01 课题由来**：课题是让 AI 深入老业务系统，而非上一套新系统；上新系统是采购问题，深入老系统是工程问题；IT 团队先卡在概念层：谁调用谁、标准和工具是哪些
- **02 五概念角色**：Model 是大脑负责想，碰不到数据库里的数据；Agent 是一套系统，Model 只是它的一个部件；Skill 是工程化、可版本管理的 SOP，区别于随口叮嘱的 Prompt；CLI 管工具怎么执行，MCP 管怎么把工具标准地暴露给 AI
- **03 协同接力**：一次调用是接力：Agent 规划→Skill→MCP Server→老系统 API→原路返回；传统微服务、OpenAPI、Wiki、SDK 可映射到 Agent、MCP、Skill、CLI
- **04 改造三路径**：先体检四维度：API 开放、文档、鉴权、变更风险；API 封装 MCP 为首选，封装层只做协议转换不碰业务逻辑；CLI 包装要求输出结构化，最好是 JSON；排序用使用频次-业务价值四象限，从高频低风险有 API 的系统入手
- **05 MCP 实现分类**：官方标准传输只有 stdio 和 Streamable HTTP 两种；SSE 从 2025 年 3 月起被取代，只做存量迁移；社区扩展有 WebSocket、WebRTC、进程内 RPC，按需采用；MCP Gateway、Proxy、托管属于部署架构，与传输方式无关
- **06 两个实战案例**：BS 请假销假系统走 API 封装，老系统一行代码没动；CS 的 PLM 走 CLI 包装，用客户端 API 和 SDK 包成 MCP；复用经验：先体检、封装要薄、鉴权写进项目计划
- **07 效果与复盘**：客户好评，三档路径决策树可直接套用；复盘的及格标准：对方听完觉得这事自己就能干

### 带走三句话

1. 五概念别背名词：Model 是大脑，Agent 是系统，Skill 是 SOP，MCP 管暴露标准，CLI 管执行
2. 老系统接入没有银弹：先体检，再按 API 封装、CLI 包装、RPA 兜底选路径，封装越薄越稳
3. MCP 官方传输只有 stdio 与 Streamable HTTP，SSE 是历史方案，见到它只做存量迁移

### 知识卡

跨全文挖掘 10 张知识卡（数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
