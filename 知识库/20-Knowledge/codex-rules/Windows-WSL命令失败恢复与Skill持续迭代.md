---
id: 20260712-commit-windows-wsl-failure-recovery
type: knowledge
title: Windows WSL 命令失败恢复与 Skill 持续迭代
aliases:
  - Windows 命令失败案例库
  - PowerShell WSL 恢复闭环
tags:
  - Codex
  - Windows
  - PowerShell
  - WSL
  - skill-evolution
status: active
created: 2026-07-12
updated: 2026-07-12
source_sessions: []
source_refs:
  - C:/Users/luode/.codex/skills/windows-wsl-execution-rules/SKILL.md
related:
  - 20-Knowledge/codex-rules/仓库总规则|仓库总规则
topics:
  - 命令失败恢复
  - 跨环境执行
  - Skill 持续迭代
confidence: high
---

# Windows WSL 命令失败恢复与 Skill 持续迭代

## 定义

Windows、PowerShell、Git Bash、WSL 和 Linux 执行过程中，命令失败不直接反复重试，而是保留脱敏证据、确认根因、选择最小恢复路径、用同一输入验证，并把已验证且可复用的经验回写到目标 Skill 的案例库。

## 操作规则

- 先区分 shell 语法、路径语境、工具 interop、JSON、编码和权限根因。
- 只执行能区分根因的窄诊断；同一失败假设最多无变化重试一次。
- 验证不能只看退出码，还要检查输出、产物、工作目录和 local 配置。
- 案例必须脱敏、去重，并明确适用边界；未确认或一次性失败不升级为长期规则。

## 证据

- windows-wsl-execution-rules/references/command-failure-recovery.md：案例模板、恢复路由和 2026-07-12 已验证案例。
- 本次提交前已通过字典刷新、UTF-8 回读、内容检查和 git diff --check。

## 关联

- [[20-Knowledge/codex-rules/仓库总规则|仓库总规则]]


## 2026-07-12 PowerShell 环境准备决策

- Windows 专项入口已验证使用 PowerShell 7.6.3；Windows Terminal 用户级 defaultProfile 指向唯一受管 PowerShell 7 profile。Windows PowerShell 5.1 保留作旧脚本兼容，不替换 powershell.exe。
- Windows PowerShell 5.1 与 PowerShell 7 的用户级 profile 均通过 UTF-8 code page 65001 探针。
- Windows 侧已验证 rg、fd、fzf、jq、yq、bat、eza、delta、just、sd、zoxide、wget2、aria2c、gsudo 和 Git；7-Zip 与 tlrc 因当前用户非管理员且安装器要求提升权限，保留为权限阻断，不自动绕过 UAC。
- WSL 原生 PATH 不含 /mnt；上述 Windows 工具不视为 WSL 工具。WSL 逐项 command -v 复核均为 MISSING，未发现 Windows .exe 误走路径。
- Windows PowerShell 环境 Skill 的 Audit、JSONC Apply、重复 Apply 幂等、Rollback 测试均通过；字典刷新后 implemented_total=83、planned_missing=0。

## 关联

- [[20-Knowledge/codex-rules/仓库总规则|仓库总规则]]

## 2026-07-12 Windows PowerShell 自动失败迭代

- 新会话先运行 `SessionEnsure`：用户级 TTL marker、原子锁和 Apply journal 校验共同决定是否可跳过重复检查；不完整 Apply 写 `complete=false`，7z/tlrc 等权限阻断不伪造成功。
- PowerShell `CommandNotFoundException`、`command-not-found`、Windows `not recognized` 归 `windows-powershell-environment-rules`；WSL 原生 `127`、`command -v` 缺失和 `/mnt/*.exe` 误用仍归 `windows-wsl-execution-rules`。
- `RecoverCommand`/wrapper 仅接受 canonical manifest 映射或调用方提供的精确 `PackageId`，不执行 `winget search` 猜包；成功后非 canonical 工具写入用户级 `discovered-tools.json`，带 `verified=true`、ID/命令/source 白名单校验，canonical manifest 不运行时自改。
- `failure-cases.json` 采用 UTF-8 原子替换、去重、限长和脱敏；当前 runtime 没有任意 agent shell 调用的全局拦截，自动恢复范围仅限显式 wrapper 或未来接入的 runtime hook。

## 关联

- [[20-Knowledge/codex-rules/执行失败持续学习与主动预防|执行失败持续学习与主动预防]]
