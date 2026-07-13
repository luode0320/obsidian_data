---
id: 20260712-imagegen-error-case-evolution
type: knowledge
title: Imagegen 错误案例持续演进
aliases:
  - gpt-image-2 错误案例库
  - 生图失败经验回写
tags:
  - Codex
  - imagegen
  - gpt-image-2
  - skill-evolution
status: active
created: 2026-07-12
updated: 2026-07-12
source_sessions: []
source_refs:
  - C:/Users/luode/.codex/skills/imagegen/SKILL.md
  - C:/Users/luode/.codex/skills/imagegen/references/error-casebook.md
related:
  - 20-Knowledge/codex-rules/Windows-WSL命令失败恢复与Skill持续迭代|Windows WSL 命令失败恢复与 Skill 持续迭代
  - 20-Knowledge/codex-rules/luode-skills-仓库概览|luode-skills 仓库概览
topics:
  - 生图错误诊断
  - 案例库维护
  - 脱敏验证
confidence: high
---

# Imagegen 错误案例持续演进

## 定义

权威 imagegen skill 使用 references/error-casebook.md 记录已经复现、解决、验证并脱敏的生图错误；案例库用于复用排障经验，不覆盖当前代码、官方参数约束或用户当前指令。

## 操作规则

- 失败先按参数、环境、鉴权、网络限流、模型能力和输出校验分类，再检索案例。
- 只执行已验证方案；涉及换模型、换路径、真透明回退或改脚本时遵守用户确认规则。
- 解决后使用 dry-run、check 或等价本地验证；未验证错误不得标记为可执行案例。
- 获得维护授权后才回写，回写前按案例 ID、错误特征和适用模型去重；旧方案失效时标记为 superseded。
- 禁止写入 API key、token、密码、完整响应、完整 prompt、输入图片内容和未经脱敏路径。

## 首批案例

- gpt-image-2 尺寸约束、透明背景不支持、input_fidelity 不支持。
- CLI 依赖缺失、无可用鉴权通道。
- generate-batch 的限流和瞬态网络错误，按最大 3 次尝试处理；单图路径暂不自动重试。

## 证据

- 仓库规则：C:/Users/luode/.codex/skills/imagegen/SKILL.md。
- 案例库：C:/Users/luode/.codex/skills/imagegen/references/error-casebook.md。
- 本轮验证：合法/非法参数 dry-run、入口 check、重试伪客户端、UTF-8、敏感信息和字典校验均通过。
