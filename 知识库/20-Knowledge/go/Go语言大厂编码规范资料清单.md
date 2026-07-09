---
id: 20260709-000000-go-style-guides
type: knowledge
title: Go 语言大厂编码规范资料清单
aliases:
  - Go style guide
  - Go编码规范
  - Uber Go Style Guide
  - Google Go Style Guide
tags:
  - go
  - style-guide
  - luode-skills
  - golang-patterns
status: active
created: 2026-07-09
updated: 2026-07-09
source_sessions: []
source_refs:
  - https://github.com/uber-go/guide
  - https://google.github.io/styleguide/go/
  - https://github.com/google/styleguide
  - https://github.com/cockroachdb/cockroach/blob/master/docs/style.md
  - https://github.com/golang/go/wiki/CodeReviewComments
  - https://github.com/xxjwxc/uber_go_guide_cn
related:
  - [[20-Knowledge/codex-rules/luode-skills-仓库概览|luode-skills 仓库概览]]
entities:
  - luode-skills
  - golang-patterns
topics:
  - Go 编码规范
  - 代码风格
confidence: high
---

# Go 语言大厂编码规范资料清单

## 定义

通过 WebSearch 核实过的、真实存在且公开可访问的大厂/知名组织 Go 语言编码规范资料，用于评估是否吸收进本仓库 golang-patterns skill。

## 证据（已核实存在的公开规范）

| 来源 | 仓库/地址 | 说明 |
|---|---|---|
| Uber | https://github.com/uber-go/guide | 业界引用最多的 Go 规范，一份完整 style.md，覆盖行长度、错误处理、命名、测试等。有中文翻译 https://github.com/xxjwxc/uber_go_guide_cn |
| Google | https://google.github.io/styleguide/go/ ，源码在 https://github.com/google/styleguide 的 go 目录 | 官方 Style Guide + Style Decisions + Best Practices 三部分，Google 内部 Go 代码规范 |
| CockroachDB | https://github.com/cockroachdb/cockroach/blob/master/docs/style.md | 基于 Uber 规范二次扩展，附带 crlfmt 格式化工具（cockroachdb/crlfmt），有具体的枚举/错误处理/测试约定 |
| Go 官方团队 | https://github.com/golang/go/wiki/CodeReviewComments | Go 团队自己维护的 Code Review 常见意见清单，非大厂但权威 |

## 未找到的部分

- 全网检索确认：字节跳动、腾讯、哔哩哔哩、滴滴等中国大厂没有公开发布独立的Go编码规范仓库；只有开源框架（字节 Kitex/CloudWeGo、腾讯 TarsGo、B站 kratos/openbilibili-go-common），规范隐含在代码里而非文档化。
- 阿里云开发者社区有博客文章讨论 Go 编码规范，但未发现阿里官方独立开源仓库。

## 关联

- 若要吸收进本仓库，落点应是直接增补现有 golang-patterns skill 的 SKILL.md（该 skill 已存在，不同于 Java 场景需要新建 skill），优先对比 Uber 和 Google 两份权威规范与当前 skill 内容的差异。
- 参见 luode-skills 仓库概览 笔记中"代码风格类 skill 架构模式"小节。



## 深入沉淀（2026-07-09）

- Uber Go 语言编码规范中文版已完整拉取并结构化摘要，详见 [[50-Sources/repo/Uber-Go语言编码规范中文版|Uber-Go语言编码规范中文版]]。
- 对照 golang-patterns 现状的采纳决策矩阵，详见 [[20-Knowledge/go/golang-patterns合并候选清单|golang-patterns合并候选清单]]。