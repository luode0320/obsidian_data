# Windows PowerShell App Execution Alias 与 error 1920

- 状态: 已验证
- 适用: Windows 上通过候选路径启动或探测 PowerShell 的工具。
- 现象: 直接读取或哈希 `%LOCALAPPDATA%\Microsoft\WindowsApps\pwsh.exe` 时，可能出现 Windows error 1920（系统无法访问文件）。
- 根因: 该路径是零字节 `ReparsePoint` App Execution Alias，不是应执行或校验的 PowerShell 二进制。
- 修复: 候选解析必须跳过 WindowsApps alias / reparse point；Godot 插件调用 Windows PowerShell 时固定使用 `%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe` 并传 `-NoProfile -NonInteractive`。
- 验证: System32 PowerShell 5.1 与真实 pwsh 7.6.3 的非交互子进程均退出 0；项目 `PortResolver.windows_powershell_candidates()` 只返回 System32 绝对路径。
- 边界: 不删除 alias、不改 PATH、不重装 PowerShell；只修正错误的候选选择或文件探测逻辑。