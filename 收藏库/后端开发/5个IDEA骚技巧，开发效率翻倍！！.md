---
title: "5个IDEA骚技巧，开发效率翻倍！！"
url: "https://mp.weixin.qq.com/s?__biz=Mzg2OTA0Njk0OA==&mid=2247538859&idx=1&sn=22216361a7110659c2afac9da82d2c34"
source: "微信公众平台"
published: "2023-09-24"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发]
lang: zh
status: ok
阅读页: "阅读页/后端开发/5个IDEA骚技巧，开发效率翻倍！！.html"
---

# 5个IDEA骚技巧，开发效率翻倍！！

> 一句话定位：IDEA 五个提效技巧：注释对齐、快速建包、复制类代码、多实例启动、分支比较。

## 摘要

本文介绍博主常用的五个 IntelliJ IDEA 使用技巧。一是注释对齐：在 Editor -> Code Style -> Java -> Code Generation 的 Comment code 区域，取消 Line comment at first column，勾选 Add a space at line comment start 与 Enforce on reformat，使 ctrl + / 注释对齐代码并留空格。二是快速创建包目录/文件夹：新建类用 . 拼接路径、新建文件用 / 拼接路径即可顺带创建。三是快速复制类代码生成 Java 类：复制代码后选中包目录 ctrl + v 粘贴。四是多实例启动：Modify options 勾选 Alow multiple instance，并用 --server.port 指定不同端口。五是分支比较：Git -> Compare with Branch 选 master 查看差异，便于上线前 review。示例基于 IDEA 2023.2。

## 要点

- 注释对齐：调整 Code Generation 的 Comment code 三项设置，让 ctrl + / 注释对齐代码并留空格。
- 快速创建包目录：新建 Java 类时用 . 拼接路径，如 controller.TestController。
- 快速创建文件夹：新建文件时用 / 拼接路径，如 template/test.csv。
- 快速复制类代码：选中包目录后 ctrl + v 粘贴即可生成 Java 类。
- 多实例启动：Modify options 勾选 Alow multiple instance，用 --server.port 指定不同端口。
- 分支比较：Git -> Compare with Branch 选 master，左侧列出文件差异便于 review。
- 技巧基于 IDEA 2023.2，过低版本界面可能不同。

## 提炼

> 一句话定位：IDEA 五个提效技巧：注释对齐、快速建包、复制类代码、多实例启动、分支比较。

### 分节提炼

- **01 注释对齐**：快捷键 ctrl + / 注释默认顶格，与代码不对齐。；在 Editor -> Code Style -> Java -> Code Generation 的 Comment code 区域调整三项设置。；调整后注释自动对齐当前代码并留一个空格。
- **02 快速创建包目录/文件夹**：新建 Java 类时用 . 拼接包路径，如 controller.TestController，包目录自动创建。；新建文件时用 / 拼接路径，如 template/test.csv，文件夹自动创建。
- **03 快速复制类代码生成 Java 类**：复制外部 demo 类代码后，选中目标包目录直接 ctrl + v 即可生成 Java 类。；免去手动新建类再拷贝代码的重复操作。
- **04 多实例启动**：在服务配置 Modify options 勾选 Alow multiple instance 启用多实例。；勾选 Program arguments 并用 --server.port=端口 区分实例，Spring 项目适用。；Modify options 里还有 add VM options 等常用功能。
- **05 分支比较**：右键项目根目录 Git -> Compare with Branch 选择分支比较。；选择 master 后，文件差异列在左侧，点击文件查看具体差异。；适合上线前多人迭代的代码 review。
- **06 总结**：五个技巧覆盖注释、建包、复制类、多实例与分支比较。

### 带走三句话

1. 调好 Comment code 三项设置，ctrl + / 注释就能对齐代码并留空格。
2. 新建类/文件时用 . 或 / 拼接路径，可顺带创建包目录或文件夹。
3. 多实例启动配端口、分支比较看差异，本地调试与上线 review 更高效。

### 知识卡

跨全文挖掘 6 张知识卡（代码模式 / 易错点 / 结论），带原文锚点，完整内容见阅读页提炼层。
