---
id: 20260708-000000-luode-skills-overview
type: knowledge
title: luode-skills 仓库概览
aliases:
  - luode-skills
  - 当前项目概览
  - 项目概览
tags:
  - codex/rules
  - repository-overview
  - obsidian/knowledge-flow
status: active
created: 2026-07-08
updated: 2026-07-08
source_sessions: []
source_refs:
  - D:/luode/luode-skills/AGENTS.md
  - D:/luode/luode-skills/CLAUDE.md
  - D:/luode/luode-skills/PROJECT_MEMORY.md
  - D:/luode/luode-skills/PROJECT_STYLE.md
  - D:/luode/luode-skills/git-collaboration-rules/SKILL.md
  - D:/luode/luode-skills/obsidian-knowledge-flow/SKILL.md
related:
  - [[20-Knowledge/codex-rules/仓库总规则|仓库总规则]]
  - [[INDEX]]
entities:
  - luode-skills
  - Codex
  - Obsidian
topics:
  - 仓库概览
  - Git 协作
  - Obsidian 沉淀
confidence: high
---

# luode-skills 仓库概览

## 仓库定位

- 路径：`D:/luode/luode-skills`
- 角色：Codex / Claude 的团队研发规则仓库
- 主要内容：`AGENTS.md`、`CLAUDE.md`、`PROJECT_MEMORY.md`、`PROJECT_STYLE.md`、各类 `skill` 及其 references

## 主要入口

- [[20-Knowledge/codex-rules/仓库总规则|仓库总规则]]：仓库级硬规则与选择性 Obsidian 触发总入口。
- `D:/luode/luode-skills/编码skill.md`：编码习惯与 skill 体系总纲。
- `D:/luode/luode-skills/git-collaboration-rules/SKILL.md`：Git 协作与提交流程规则。
- `D:/luode/luode-skills/obsidian-knowledge-flow/SKILL.md`：Obsidian 知识流选择性默认触发规则。
- `D:/luode/luode-skills/PROJECT_MEMORY.md`：长期项目记忆。
- `D:/luode/luode-skills/PROJECT_STYLE.md`：长期风格记忆。

## 联动口径

- 当本项目出现提交、推送、PR 收口或交付说明准备，且本轮形成可复用事实、决策、流程、定义、偏好、来源或调试经验时，优先先检索 Vault 中是否已有承接笔记，再做 `Obsidian:沉淀`。
- `Obsidian:沉淀` 只负责知识捕获，不代表获得 `git commit` 或 `git push` 授权。
- 新增项目事实、规则口径或长期偏好时，优先回写 `PROJECT_MEMORY.md` 和对应 Vault 笔记，保持仓库规则和知识库同步。

## 检索关键词

- `luode-skills`
- `AGENTS.md`
- `CLAUDE.md`
- `Git 提交`
- `Obsidian:沉淀`
- `仓库总规则`

- `Git 收口联动沉淀`
- `提交前知识捕获`

## 工具落点规则

- util 仅放与当前项目无关、脱离项目上下文仍成立的通用工具。
- common/util 仅放可复用但依赖当前项目文件、路径、配置、命名约定或目录结构的工具。
- 引用项目文件或项目约定的复用工具不要放进独立 util，优先归入 common/util。



## 代码风格类 skill 架构模式（2026-07-09 调研）

- 本仓库代码风格类 skill 遵循三层模式：语言无关通用骨架（naming-rules、code-readability-rules、code-minimal-change-rules、common-util-rules、comment-placement-granularity-rules、comment-completion-gate-rules、chinese-comment-rules）+ 单语言完整规范 skill（golang-patterns，目前唯一的 Go 专属完整规范类 skill）+ 按语言拆分 references 的先例（package-structure-rules 下有 go-package-layout.md / java-layer-layout.md / node-python-module-layout.md）。
- logging-trace-rules、database-query-rules、api-endpoint-rules / api-request-rules / api-response-rules 目前偏 Go+Gin+GORM 具体实现细节，尚未按语言拆分。
- 全仓检索确认此前未吸收过"阿里巴巴Java开发手册"/P3C 规约（无匹配），仓库也没有 Java 专属完整规范 skill（golang-patterns 的对等角色空缺）。
- 结论口径：若要吸收阿里巴巴Java开发手册，不适合整体塞进语言无关骨架 skill（会引入大量 Java 语法专属条目，破坏现有分层）；更合适的落点是新增 java-patterns skill（对标 golang-patterns），系统性吸收手册里 Java 专属章节（命名/常量/OOP/集合/并发/异常/日志/MySQL/工程结构）；手册里少量真正语言中立的设计原则可挑选性补充进现有骨架 skill，而非整份文档搬运。


## Go 编码规范资料清单（2026-07-09）

- 已核实存在的公开大厂 Go 编码规范：Uber、Google、CockroachDB，详见 [[20-Knowledge/go/Go语言大厂编码规范资料清单|Go语言大厂编码规范资料清单]]。
- 中国大厂（字节跳动/腾讯/哔哩哔哩/滴滴）没有公开发布独立 Go 编码规范仓库，只有开源框架代码。
- 与阿里巴巴Java开发手册场景不同：golang-patterns skill 已存在，若要吸收应直接增补该 skill，不需要新建 skill。