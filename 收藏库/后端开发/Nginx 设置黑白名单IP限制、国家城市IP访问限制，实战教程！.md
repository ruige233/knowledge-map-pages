---
title: "Nginx 设置黑白名单IP限制、国家城市IP访问限制，实战教程！"
url: "https://mp.weixin.qq.com/s?__biz=MzI3MDM0MzAyMg==&mid=2247503359&idx=1&sn=01701fc75e9beb23050fc64dfc253f1d"
source: "微信公众平台"
published: "2026-11-01"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发]
lang: zh
status: ok
阅读页: "阅读页/后端开发/Nginx 设置黑白名单IP限制、国家城市IP访问限制，实战教程！.html"
---

# Nginx 设置黑白名单IP限制、国家城市IP访问限制，实战教程！

> 一句话定位：Nginx IP/地区访问限制：allow/deny、geo、geoip 实战

## 摘要

本文讲 Nginx 限制 IP 访问：黑白名单可用 allow/deny 直接配置或 include 文件加载；也可用 geo 模块按 IP 段定义变量、在 location 中 if 判断返回 403。国家地区限制需启用 geoip 模块，下载 dat 格式 IP 库，用 geoip_country、geoip_city 加载后按 $geoip_country_code 等变量判断。文中附安装命令与变量含义，并建议返回 503/504 以免暴露 nginx 信息。

## 要点

- nginx 默认加载 ngx_http_access_module，deny/allow 指令可直接做 IP 黑白名单。
- allow/deny 可通过 include 引入 whitelist.conf 等文件集中管理白名单 IP。
- ngx_http_geo_module 按 IP 段给变量赋值，配合 if 判断实现黑白名单，并支持 include 文件。
- geo 模块定义的变量值可自定义为 uk/us 等，配合 upstream 实现按地区负载均衡。
- 国家地区限制需额外启用 ngx_http_geoip_module：ubuntu 装 nginx-extras，centos 装 nginx-module-geoip。
- geoip 模块依赖 MaxMind 的 dat 格式 IP 库，可从 miyuru.lk 下载 country/city 版本。
- 配置中用 geoip_country、geoip_city 指定库路径，再用 $geoip_country_code 等变量判断访问来源。
- 限制请求建议返回 503/504，避免暴露 nginx 信息。

## 提炼

> 一句话定位：Nginx IP/地区访问限制：allow/deny、geo、geoip 实战

### 分节提炼

- **01 allow/deny 黑白名单**：deny/allow 属 ngx_http_access_module，nginx 默认加载，可直接使用。；黑名单不用加 allow all，因为默认就是允许所有访问。；白名单可 include whitelist.conf 从文件读取，相对路径默认在 /etc/nginx/ 下。；可对特定目录 location /tree/list 单独设置目录级限制。
- **02 geo 模块 IP 限制**：geo 参数需放在 http 模块中，位置在 server 模块外即可。；按 IP 段给变量赋值，default 设默认值，if 判断变量值来放行或拒绝。；可 include ip_white.conf 从文件读取 IP 映射。
- **03 geo 负载均衡扩展**：geo 变量值可自定义为 uk、us、default 等字母，按 IP 段映射地区。；配合 upstream 将不同 IP 段分流到对应地区服务器，实现地区负载均衡。
- **04 安装 geoip 模块**：geoip 模块默认不构建，编译时需 --with-http_geoip_module 启用。；ubuntu 直接 apt install nginx-extras，centos 用 yum install nginx-module-geoip。
- **05 下载 IP 数据库**：geoip 依赖 dat 格式 IP 库，所有数据从库中读取。；MaxMind 官方已停止支持 dat 库，可用老版本或第三方下载，数据存在误差。；分别下载 country、city 版本并移动到 /etc/nginx/ 目录。
- **06 geoip 配置与变量**：geoip_country、geoip_city 指令指定 dat 库路径。；if ($geoip_country_code = CN) { return 403; } 可按国家码拒绝访问。；国家变量有 code/code3/name；城市变量有 city/region/postal_code/latitude/longitude 等。

### 带走三句话

1. IP 黑白名单优先用 allow/deny，简单直接；需要按 IP 段或地区规则时再上 geo/geoip 模块。
2. 被限制的请求尽量返回 503/504，避免 403 暴露 nginx 版本与拦截原因。
3. geoip 依赖 dat 库，而 MaxMind 已停更该格式，生产环境需评估数据时效与误差。

### 知识卡

跨全文挖掘 10 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
