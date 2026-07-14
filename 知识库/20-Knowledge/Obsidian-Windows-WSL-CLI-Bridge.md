---
status: active
source: CYCLE-OBS-03
project_id: windows://d/luode/luode-skills
---

# Obsidian Windows/WSL CLI bridge 固定执行边界

- Windows 与 WSL 的 Obsidian 检索、创建、追加和读取统一经公开 bridge，最终调用 Windows 官方 CLI。
- 唯一 vault 根为 `D:\obsidian_data`；`知识库/` 是 vault 内路径前缀，selector 按注册根动态唯一解析。
- WSL 使用 PowerShell interop，不安装原生 Linux CLI，不使用 vault 文件系统 fallback。
- 写入必须有 `verified=true` 的 CLI readback；长正文按自然换行分块，append 的隐含换行需纳入契约测试。
- 应用恢复最多隐藏启动一次并有限重试；timeout、interop、selector、path 和 legacy nested root 失败均返回稳定错误码。

验证证据：TASK-OBS-08/09/10/11/12，35/35 离线回归，长正文与 append 双端 readback/hash 一致。
