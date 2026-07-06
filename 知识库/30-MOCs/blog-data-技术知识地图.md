---
id: 20260706-234520-blog-data-tech-map
type: moc
title: blog-data 技术知识地图
aliases:
  - F blog data 技术知识地图
  - blog data 技术笔记索引
  - data vault 技术索引
tags:
  - moc/technical-knowledge
  - obsidian/import
  - blog-data
status: active
created: 2026-07-06
updated: 2026-07-06
source_sessions: []
source_refs:
  - F:/blog/data
related:
  - [[50-Sources/vaults/blog-data-vault|F blog data Obsidian vault]]
entities:
  - Obsidian
topics:
  - 前端
  - 后端
  - 数据库
  - 区块链
  - 服务器
  - 算法
confidence: high
---

# blog-data 技术知识地图

> [!NOTE]
> 本地图把源 vault `F:/blog/data` 的 1110 篇 Markdown 按主题纳入当前知识库检索入口。它是第一层沉淀：先固化结构、主题和后续逐篇抽取边界。

## 入口

- 来源说明：[[50-Sources/vaults/blog-data-vault|F blog data Obsidian vault]]
- 源 vault：`data`
- 源路径：`F:/blog/data`
- 目标 vault：`D:/obsidian_data/知识库`

## 核心笔记

| 主题 | 源目录 | 数量 | 代表路径 |
| --- | --- | ---: | --- |
| 前端 | `1.前端` | 161 | `1.HTML`、`2.JavaScript`、`3.CSS`、`4.React`、`5.Vue` |
| 后端 | `2.后端` | 339 | `1.Java`、`2.Go`、`3.AI` |
| 数据库 | `3.数据库` | 81 | `1.MySQL关系型数据库`、`2.Nebula图数据库`、`3.Redis缓存数据库`、`4.MongoDB文档数据库` |
| 中间件 | `4.中间件` | 15 | `Apifox`、`Docker`、`Kafka` |
| 区块链 | `5.区块链` | 305 | `基础概念`、`Eth`、`Fabric`、`共识算法`、`钱包`、`事件合约` |
| 服务器 | `6.服务器` | 111 | `操作系统`、`Linux命令`、`部署基础`、`blog博客系统`、`内网组网` |
| 算法 | `7.算法` | 58 | 算法基础和题目沉淀 |
| 设计模式 | `8.设计模式` | 38 | 常见设计模式 |
| 版本控制 | `9.版本控制` | 2 | Git |

## 主题路线

```mermaid
flowchart TD
  A[blog-data] --> B[前端]
  A --> C[后端]
  A --> D[数据库]
  A --> E[中间件]
  A --> F[区块链]
  A --> G[服务器]
  A --> H[算法与设计模式]
  C --> C1[Java]
  C --> C2[Go]
  F --> F1[Ethereum]
  F --> F2[Solidity]
  F --> F3[钱包]
  G --> G1[Linux]
  G --> G2[部署与服务]
```

## 检索提示

- 查前端：搜索 `前端`、`JavaScript`、`React`、`Vue`、`CSS`。
- 查后端：搜索 `Java`、`Go`、`Spring`、`JVM`、`并发`、`MyBatis`。
- 查数据库：搜索 `MySQL`、`Redis`、`MongoDB`、`Nebula`。
- 查区块链：搜索 `Ethereum`、`Solidity`、`Hardhat`、`Geth`、`Fabric`、`钱包`。
- 查服务器：搜索 `Linux`、`Docker`、`部署`、`frp`、`nacos`、`VPN`。

## 待整理

- [ ] 对高频主题逐篇抽取定义、流程、排障经验和代码模板。
- [ ] 对钱包、服务器、备份、VPN 等敏感主题先做脱敏策略，再决定是否沉淀正文。
- [ ] 为 Java、Go、Solidity、Linux 命令等大主题建立二级 MOC。