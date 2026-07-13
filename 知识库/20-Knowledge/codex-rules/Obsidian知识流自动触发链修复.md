---
id: 20260707-192847-obsidian-skill-auto-trigger
type: knowledge
title: Obsidian 知识流自动触发链修复
aliases:
  - obsidian-knowledge-flow 自动触发
  - Obsidian 选择性默认
  - luode-skills 记忆域
  - skill 自动命中
tags:
  - codex
  - skills
  - obsidian
  - luode-skills
status: active
created: 2026-07-07
updated: 2026-07-08
source_sessions: []
source_refs:
  - D:/luode/luode-skills
related: []
entities:
  - luode-skills
topics:
  - Codex skills
  - Obsidian 知识流
confidence: high
---

# Obsidian 知识流自动触发链修复

## 定义

obsidian-knowledge-flow 在 luode-skills 中采用选择性默认触发：仓库任务每轮先输出 Obsidian:<检索/沉淀/不适用/阻断>，只有历史依赖才检索，只有形成可复用事实、决策、流程或调试经验时才沉淀。

## 根因

此前一天没有自动触发，核心原因是 obsidian-knowledge-flow 被字典归类为扩展种子，而不是记忆域已实现 skill；上游 skill-hit-check-rules 与仓库规则也没有强制做 Obsidian 选择性默认判断。

## 决策

采用选择性默认策略，不把 Obsidian 变成每轮强制读写；普通任务判定不适用，历史问题判定检索，阶段收口有长期价值时判定沉淀，CLI 或 vault 不可用时判定阻断。

## 证据

- 仓库规则已要求仓库任务输出 Obsidian:<检索/沉淀/不适用/阻断>。
- skill-hit-check-rules 已把检索/沉淀与 obsidian-knowledge-flow 联动。
- skill 字典生成结果显示 obsidian-knowledge-flow 位于记忆域，已实现总数为 83，计划缺失为 0。

## 关联

- 仓库：D:/luode/luode-skills
- 关键文件：AGENTS.md、CLAUDE.md、skill-hit-check-rules/SKILL.md、obsidian-knowledge-flow/SKILL.md、编码skill.md、字典.md

## 迁移

- 原旧位置：D:\obsidian_data\20-Knowledge\codex-skills\
- 现已归位：D:\obsidian_data\知识库\20-Knowledge\codex-rules\
- 这篇笔记属于知识库长期内容，不再保留在旧根目录下。



## 2026-07-11 更新：项目四件套记忆闭环

- 项目本地启动上下文固定为父目录平台规则 -> PROJECT_CURRENT.md -> PROJECT_MEMORY.md；缺失时先创建。
- PROJECT_CURRENT.md 覆盖维护当前状态且不超过 51,200 字节，PROJECT_MEMORY.md 只保存稳定规则与关键决策，PROJECT_HISTORY.md 只追加并默认不读。
- 项目本地 Markdown 与 Obsidian vault 分层；vault 仍只在跨项目历史或可复用知识场景通过 CLI 选择性检索或沉淀。