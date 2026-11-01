---
title: "七张图解锁Mybatis整体脉络，让你轻松拿捏面试官"
url: "https://mp.weixin.qq.com/s?__biz=MzU4MDUyMDQyNQ==&mid=2247520633&idx=2&sn=311879989cf480583886ba245ec950ea"
source: "微信公众平台"
published: "2023-10-30"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发, 面试]
lang: zh
status: ok
阅读页: "阅读页/后端开发/七张图解锁Mybatis整体脉络，让你轻松拿捏面试官.html"
---

# 七张图解锁Mybatis整体脉络，让你轻松拿捏面试官

> 一句话定位：用七张图梳理 MyBatis 架构分层与 SQL 执行全流程

## 摘要

MyBatis 是一款轻量稳定的 ORM 框架，负责 Java 对象与关系数据库的映射，提供 SQL 映射配置、动态 SQL、参数与结果集映射、事务管理、连接池集成、二级缓存等能力。架构分基础支持层、核心处理层与接口层，使用主线为：定义接口并配置 XML、加载解析配置、生成代理并执行 SQL。配置解析由继承 BaseBuilder 的 XMLConfigBuilder、XMLMapperBuilder、XMLStatementBuilder 完成，结果存入 Configuration；接口代理经 JDK 动态代理构建，涉及 MapperProxyFactory、MapperRegistry、MapperProxy；SQL 执行底层即原生 JDBC 的创建语句、执行查询、处理结果。全流程总结为创建 SqlSessionFactory、创建 SqlSession、执行 SQL、解析 SQL、执行与结果映射。

## 要点

- MyBatis 是轻量稳定的 ORM 框架，负责 Java 对象与关系数据库之间的映射。
- 核心功能包括 SQL 映射配置、动态 SQL、参数映射、结果集映射、事务管理、连接池集成与二级缓存。
- 架构分为基础支持层、核心处理层、接口层三层。
- 使用主线：定义接口并配置 XML → 加载解析配置 → 生成接口代理并执行 SQL。
- 配置解析由 XMLConfigBuilder、XMLMapperBuilder、XMLStatementBuilder 完成，均继承 BaseBuilder，结果存入 Configuration。
- 接口代理通过 JDK 动态代理生成，涉及 MapperProxyFactory、MapperRegistry、MapperProxy。
- SQL 执行底层是原生 JDBC 逻辑：创建语句、执行查询、处理 ResultSet。
- 完整流程：创建 SqlSessionFactory → 创建 SqlSession → 执行 SQL → 解析 SQL → 执行 → 结果映射。

## 提炼

> 一句话定位：用七张图梳理 MyBatis 架构分层与 SQL 执行全流程

### 分节提炼

- **01 前言**：MyBatis 是轻量稳定的 ORM 框架，负责 Java 对象与关系数据库的映射；七大功能：SQL 映射配置、动态 SQL、参数/结果集映射、事务管理、连接池集成、二级缓存；SQL 映射配置将 SQL 与 Java 代码分离，提升可维护性
- **02 总览MyBatis**：架构三层：基础支持层、核心处理层、接口层；使用主线：定义接口配 XML → 加载解析配置 → 生成代理执行 SQL；配置解析属基础支持层，代理与 SQL 执行属核心处理层；连接池与事务分别由数据源模块、事务管理模块承担
- **03 配置文件解析**：加载 mybatis-config.xml 并创建 Configuration 对象；XML 解析器解析数据源、插件、类型别名、缓存等全局配置；XMLConfigBuilder、XMLMapperBuilder、XMLStatementBuilder 继承 BaseBuilder 各司其职；调用 SqlSessionFactory.build 加载解析，结果存入 Configuration
- **04 代理构建**：通过 SqlSession.getMapper 构建接口代理类；涉及组件：MapperProxyFactory、MapperRegistry、MapperProxy；本质是 JDK 动态代理返回接口实现实例
- **05 SQL 执行**：底层逻辑是原生 JDBC 操作数据库的一套流程；创建 Statement、PreparedStatement 或 CallableStatement 执行不同类型 SQL；通过 ResultSet 处理查询结果数据
- **06 总结**：创建 SqlSessionFactory：读取主配置，负责创建连接与 SqlSession；创建 SqlSession：代表一次会话，通常每个线程创建自己的 SqlSession；执行 SQL：支持 selectOne、selectList、insert、update、delete；解析 SQL、执行 SQL 并把结果映射为 Java 对象返回

### 带走三句话

1. 记住使用主线：定义接口、配 XML、生成代理、执行 SQL，其余组件由此发散推导。
2. 配置解析、代理构建、SQL 执行分别对应 Builder 体系、JDK 动态代理与原生 JDBC 逻辑。
3. 完整流程六步：创建 SqlSessionFactory、创建 SqlSession、执行并解析 SQL、执行、结果映射。

### 知识卡

跨全文挖掘 8 张知识卡（代码模式 / 数据 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
