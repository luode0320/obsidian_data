# Godot Windows PowerShell 启动可靠性

状态: 已验证

- Godot AI 插件的 Windows 端口和进程查询必须把 `SystemRoot/System32/WindowsPowerShell/v1.0/powershell.exe` 排在 `pwsh.exe` 之前，避免 PowerShell 7 App Execution Alias 异常时先触发启动失败。
- PowerShell 子进程使用 `-NoProfile -NonInteractive -ExecutionPolicy Bypass -Command`，避免用户 Profile 和交互提示参与后台查询。
- `netstat -ano` 成功时直接返回解析结果；空闲端口不应启动任何 PowerShell 子进程。
- 已用 Godot 4.7 headless 脚本验证：空闲端口轨迹仅为 netstat，统一入口以 Windows PowerShell 5.1 返回版本主号 5 和退出码 0。
- 未确认：缺少原始调用栈，历史 Windows error 1920 的系统层原因不可精确归因。

## 2026-07-12 复验

- 当前磁盘补丁已再次通过 Godot 4.7 headless 验证：候选顺序为绝对路径、pwsh.exe、powershell.exe；空闲端口只走 netstat；统一入口返回 Windows PowerShell 主版本 5 和退出码 0。

- 2026-07-12 末次复验：Godot 4.7 headless 的 T-PS-1920 通过；空闲端口 65534 仅执行 netstat，统一 PowerShell 入口经绝对路径 Windows PowerShell 5.1 返回版本主号 5 与退出码 0。


## 2026-07-12 安装器入口补齐

- Godot AI 插件的 Windows uv 安装动作也必须复用 PortResolver.windows_powershell_candidates() 的首个绝对路径候选，并传入 -NoProfile -NonInteractive -ExecutionPolicy Bypass -Command。
- 这关闭了安装 UI 绕过端口解析器、重新依赖 App Execution Alias 的独立调用路径。
- 使用相同可执行文件和参数的子进程探针退出码为 0；未点击安装按钮，因为该操作会联网并修改用户工具环境。


## 2026-07-13 复验

- Godot 4.7 headless 再次运行 T-PS-1920 通过：候选顺序保持 System32 Windows PowerShell 5.1、pwsh.exe、powershell.exe；空闲端口 65534 仅使用 
etstat；统一入口返回版本主号 5、退出码 0。
- 另以相同非交互参数直接探测 Windows PowerShell 5.1 与已安装 PowerShell 7.6.3，均退出 0；未复现 Windows error 1920。
- 残余边界：仅当 System32 首选路径不可执行时端口解析才回退 pwsh.exe 或命令名入口；历史系统层错误仍因缺原始调用栈不可精确归因。

## 2026-07-13 文本修正

- 上一节中的命令名称以本条为准：空闲端口 65534 仅使用 netstat；统一入口以 System32 Windows PowerShell 5.1 返回版本主号 5 和退出码 0。

## 2026-07-13 加固口径更新

- 当前已验证策略改为唯一候选：SystemRoot/System32/WindowsPowerShell/v1.0/powershell.exe。不再保留 pwsh.exe 或裸 powershell.exe 回退，因此不会重新进入 App Execution Alias 解析。
- 端口查询与 Windows uv 安装入口均复用该唯一候选，并保持 -NoProfile -NonInteractive -ExecutionPolicy Bypass -Command。
- Godot 4.7 headless 加固回归已通过：候选列表仅一项、空闲端口 65534 轨迹仅为 netstat、安装入口契约已验证、PowerShell 主版本 5 且退出码 0。
- 边界：缺少 System32 Windows PowerShell 5.1 的非标准系统会明确失败；未执行联网 Install uv。


## 2026-07-13 最终复验

- 加固回归 T-PS-1920-HARDENED 再次通过：唯一候选仍是 System32 Windows PowerShell 5.1，空闲端口仅使用 netstat，安装入口复用共享候选，子进程退出码为 0。
- 插件目录裸 PowerShell 启动调用扫描为 0；本地 Godot MCP initialize 返回 HTTP 200（Godot AI 3.4.4）。
- Godot headless 退出仍输出 ObjectDB 泄漏警告；断言与进程退出码均通过，该警告不作为本修复失败证据。
- 未执行 Install uv，避免网络下载和用户环境改动。


## 2026-07-13 当前复验

- T-PS-1920-HARDENED 在 Godot 4.7 本地 headless 下再次通过：唯一候选为 System32 Windows PowerShell 5.1，空闲端口仅调用 
etstat，安装入口复用共享候选，PowerShell 版本探针退出 0。
- Windows error 1920 是 App Execution Alias 文件不可访问；不要删除 alias、调整 PATH 或重装 PowerShell。调用方应只用绝对 System32 路径，且不对 alias 做文件探测或直接启动。
- Godot 退出的 ObjectDB 泄漏警告未触发该测试任何断言，进程退出码为 0。

- 勘误：上两段中因 CLI 转义显示异常而断行的命令名均应读作 netstat；本轮真实 Godot 输出为 trace=[netstat]。