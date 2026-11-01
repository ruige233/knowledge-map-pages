---
title: "这份 CLAUDE.md 模板，让 Claude Code 写出企业级 Java 项目"
url: "https://mp.weixin.qq.com/s?__biz=MzAxNDMwMTMwMw==&mid=2247560236&idx=1&sn=35b2d9d42fa0915c9d782eb0ef753042"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "AI编程"
tags: [AI编程, Java, Claude Code]
lang: zh
status: ok
阅读页: "阅读页/AI编程/这份 CLAUDE.md 模板，让 Claude Code 写出企业级 Java 项目.html"
---

# 这份 CLAUDE.md 模板，让 Claude Code 写出企业级 Java 项目

> 一句话定位：配好 CLAUDE.md，Claude Code 才能写出企业级 Java

## 摘要

文章指出，用 Claude Code 写 Java 的多数人错在没配置 CLAUDE.md：没有它，AI 像第一天入职的新人，规范全靠猜。作者给出可复制的 Java SpringBoot CLAUDE.md 模板，锁定 Java 21、Spring Boot 3.2.x 等技术栈，规定分层、命名、统一响应 ResponseDTO 与异常处理，并列出禁止模式。配前配后代码对比说明效果，强调 Plan Mode 应占 90% 时间、规则要具体可测试，CLAUDE.md 只放 AI 无法自动执行的规则

## 要点

- CLAUDE.md 是项目根目录的 Markdown，Claude Code 每次启动自动加载为上下文。
- 模板锁定技术栈：Java 21、Spring Boot 3.2.x、Maven、JUnit 5 + Testcontainers。
- 分层固定为 controller/service/repository/model/dto/config/exception，DTO 不直接暴露 Entity。
- Controller 统一返回 ResponseEntity<ResponseDTO<T>>，校验用 @Valid，错误用 ProblemDetail 与自定义异常。
- Service 用构造器注入，@Transactional 只加实现类，禁止嵌套事务。
- Repository 用 DTO Projection、@EntityGraph/JOIN FETCH，禁止循环调库与 EAGER。
- 工作流要求非简单任务先 Plan Mode，改完必须 ./mvnw test 与 checkstyle 通过。
- 维护原则：只写 AI 无法自动执行的规则，随 Code Review 反馈持续更新。

## 提炼

> 一句话定位：配好 CLAUDE.md，Claude Code 才能写出企业级 Java

### 分节提炼

- **01 CLAUDE.md 是什么**：CLAUDE.md 放在项目根目录，Claude Code 每次启动自动加载为上下文；它是项目记忆系统，告诉 AI 技术栈、组织方式、禁止写法、问题处理；层次：根文件必加载，skills/ 按需，agents/ 放子代理
- **02 完整模板**：模板覆盖技术栈、架构、代码、工作流、禁止模式、API、Git 提交七类规范；Java 21、Spring Boot 3.2.x、JUnit 5 + Testcontainers；七层分包，DTO 不直接暴露 Entity；命名规范、统一返回 ResponseDTO、异常用 ProblemDetail 与自定义异常
- **03 怎么用起来**：四步：建 CLAUDE.md → 改包名/版本/团队规范 → 启动验证 → Git 签入共享；最少修改技术栈、包命名、命名规范、禁止模式四处；验证时让 Claude Code 写用户查询接口，观察是否按规范返回 UserDTO
- **04 配了前后的代码对比**：未配置时：直接返回 Entity、抛 RuntimeException、URL 单数、无统一响应；配置后：复数 URL、返回 ResponseDTO、调用 Service、异常交全局处理；同样的 prompt，规范文件决定生成代码是否可直接用
- **05 逐段解析**：不声明技术栈，AI 会猜 Java 版本、Spring Boot 版本、测试库，每处都要返工；Plan Mode 应覆盖 90% 时间，先规划后执行，减少返工；禁止模式最有用：规则要具体可测试，如「禁止循环里调用数据库方法」；把 Code Review 反复出现的问题写进禁止模式，生成阶段拦截
- **06 维护建议**：三个更新时机：Code Review 反复问题、新选型、规范废弃；CLAUDE.md 只放 AI 无法自动执行的规则；Checkstyle/FindBugs 能强制检查的不写进 CLAUDE.md

### 带走三句话

1. 把 CLAUDE.md 放进项目根目录，Claude Code 每次启动自动加载，成为项目记忆。
2. 规则要具体可测试：写「禁止在循环里调用数据库方法」，而不是「避免 N+1」。
3. CLAUDE.md 只写 AI 无法自动执行的规则，工具能检查的交给 Checkstyle/FindBugs。

### 知识卡

跨全文挖掘 12 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
