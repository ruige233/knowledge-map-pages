---
title: "千万级数据的全表update的正确姿势"
url: "https://mp.weixin.qq.com/s?__biz=MzI1NDQ3MjQxNA==&mid=2247511644&idx=1&sn=cdb5984bfdcdf6c55a140012cff87347"
source: "微信公众平台"
published: "2024-09-26"
archived: "2026-11-01"
category: "后端开发"
tags: [后端开发]
lang: zh
status: ok
阅读页: "阅读页/后端开发/千万级数据的全表update的正确姿势.html"
---

# 千万级数据的全表update的正确姿势

> 一句话定位：千万级数据全表update：主键游标分批+绕开buffer pool

## 摘要

本文记录一次把用户图片路径从 http 批量改成 https 的全表 update 优化过程。线上 MySQL 用 row 格式 binlog 做主从同步，亿级大表直接 update 会产生海量 binlog、拖垮从库。作者依次尝试直接 update、limit 游标分批、查 id 后用 in 批量更新，分别暴露 binlog 压力、深分页低效与 in 效率不佳的问题。最终方案：查询用 SQL_NO_CACHE 绕过 buffer pool、FORCE INDEX(PRIMARY) 强制走主键索引排序，再按排序后的 id 范围分段 update，并用接口控速、线程池并行。文章还提醒存储 path 别存协议域名前缀，并讨论 uuid 与 snowflake/自增主键的处理差异及读写缓存、change buffer 细节。

## 要点

- 亿级大表全表 update 会因 row 格式 binlog 产生海量日志，主从同步压力极大，不能直接执行。
- limit 游标分批在数据量大时近似全表扫描，即“深度分页问题”。
- 把 id 全查出来再用 in 批量更新，虽有键值预测但效率仍然低。
- 最终方案：select 用 SQL_NO_CACHE 绕开 buffer pool，FORCE INDEX(PRIMARY) 强制主键索引排序。
- 按排序后的主键 id 范围分段执行 update，可避免深分页。
- 通过接口控制刷库速率，观察主从同步、iops、内存等指标，可用线程池并行提速。
- 存储 path 路径不要存协议和域名前缀，便于后续替换且省容量。
- uuid 主键无序，需业务预先处理，无法像自增或雪花主键那样顺序刷。

## 提炼

> 一句话定位：千万级数据全表update：主键游标分批+绕开buffer pool

### 分节提炼

- **01 前言**：线上 MySQL 用 row 格式 binlog 做主从同步；亿级表全表 update 会产生大量 binlog，从库阻塞执行，风险极高
- **02 直接 update 的问题**：业务是把用户信息从 http 换 https，数据量几千 w 级别；path 类字段不要存协议和域名前缀，便于替换且省容量
- **03 深度分页问题**：limit 游标下沉到 B+ 树叶子节点向后遍历；limit 数据量大时效率接近全表扫描，即深度分页问题
- **04 in 的效率**：把 id 全查出后用 in 列表批量更新；mysql 对 in 有键值预测，但仍很低效
- **05 最终版本**：SQL_NO_CACHE 让本次查询不走 buffer pool，防冷数据污染热点缓存；FORCE INDEX(PRIMARY) 强制主键索引并排序，按 id 范围查找更新；接口控制速率，线程池并行提高更新速率上限
- **06 其他问题**：雪花算法或自增主键的记录按主键顺序插入，便于分片刷数；uuid 主键需业务预先替换，代码上线后再做全量更新
- **07 结语**：总结三点：binlog 同步问题、深分页效率、buffer pool 污染防护
- **08 群友问题**：读缓存和写缓存不一样，SQL_NO_CACHE 只影响读缓存；min/max 直接分片只有自增 id 才匀速，多线程任务分配也需保证；写缓存指 change buffer，本修改用得到

### 带走三句话

1. 全表 update 先按主键排序分段，用 id 范围替换 limit 游标，避免深分页。
2. 用 SQL_NO_CACHE 让扫描数据绕开 buffer pool，别让冷数据污染热点缓存。
3. path 类字段别存协议域名前缀，源头规避未来批量改协议的成本。

### 知识卡

跨全文挖掘 9 张知识卡（代码模式 / 数据 / 易错点 / 概念对比 / 结论），带原文锚点，完整内容见阅读页提炼层。
