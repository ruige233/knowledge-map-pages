---
title: "看了我的 MyBatis-Plus 用法，同事也开始悄悄模仿了"
url: "https://mp.weixin.qq.com/s?__biz=MzI3MDM0MzAyMg==&mid=2247503452&idx=1&sn=1e6222085f34876b15d5e8b8b7d1969c"
source: "微信公众平台"
published: "2023-12-31"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发]
lang: zh
status: ok
阅读页: "阅读页/后端开发/看了我的 MyBatis-Plus 用法，同事也开始悄悄模仿了.html"
---

# 看了我的 MyBatis-Plus 用法，同事也开始悄悄模仿了

> 一句话定位：Spring Boot 下 MyBatis-Plus 常用功能与配置汇总

## 摘要

本文面向 Spring Boot 用户汇总 MyBatis-Plus 常用用法。快速开始部分讲依赖引入、@MapperScan、一主一从动态数据源与 HikariCP 连接池配置，建议建库用 utf8mb4。使用部分覆盖实体类注解、Service/Mapper 继承式 CRUD、分页插件、逻辑删除、通用枚举 IEnum、自动填充 MetaObjectHandler、多数据源 @DS。测试部分以 user 表为例给出 controller 增删改查与分页接口。

## 要点

- 引入 mybatis-plus-boot-starter、mysql-connector-java、dynamic-datasource 三个依赖
- 启动类用 @MapperScan 扫描 Mapper 包位置
- 一主一从动态数据源配置 primary:master、strict:false
- 实体类注解 @TableName、@TableId(type=IdType.AUTO)、@TableField(value/condition/fill)
- Service 层继承 IService/ServiceImpl、Mapper 层继承 BaseMapper 即得 CRUD
- 分页需注册 MybatisPlusInterceptor 与 PaginationInnerInterceptor(DbType.MYSQL)
- 逻辑删除用全局 logic-delete-field 或 @TableLogic 注解，仅更新查询有效
- 通用枚举实现 IEnum 并配合 @JsonFormat/fastjson 序列化；自动填充需实现 MetaObjectHandler

## 提炼

> 一句话定位：Spring Boot 下 MyBatis-Plus 常用功能与配置汇总

### 分节提炼

- **01 快速开始**：引入 mybatis-plus-boot-starter 依赖；引入 mysql-connector 与 dynamic-datasource 依赖；启动类加 @MapperScan 扫描 Mapper 文件位置；建库选 utf8mb4，可存储微信昵称中的表情
- **02 使用**：实体类注解 @TableName/@TableId/@TableField 做映射；Service 继承 IService，Mapper 继承 BaseMapper 获得 CRUD；分页需注册 MybatisPlusInterceptor 与分页内部拦截器；逻辑删除、通用枚举 IEnum、自动填充 MetaObjectHandler、多数据源 @DS
- **03 测试**：建 user 表并含 sex、is_delete、create_time 等字段；controller 注入 IUserService 调增删改查与分页方法；分页接口用 new Page<>(current,size) 加 QueryWrapper

### 带走三句话

1. 三依赖：mybatis-plus-boot-starter、mysql、dynamic-datasource。
2. CRUD 靠继承：Service 走 IService，Mapper 走 BaseMapper。
3. 坑：HikariCP 不配连接池会报错，逻辑删除只对更新查询生效，枚举序列化记得 @JsonFormat。

### 知识卡

跨全文挖掘 11 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
