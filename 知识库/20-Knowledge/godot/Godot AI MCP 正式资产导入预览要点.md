---
id: 20260712-145000-godot-ai-mcp-c8-asset-preview
type: knowledge
title: Godot AI MCP 正式资产导入预览要点
aliases: [Godot MCP asset preview, C8 资产导入验证]
tags: [godot, mcp, asset-pipeline]
status: active
created: 2026-07-12
updated: 2026-07-12
source_sessions: []
source_refs: []
related: []
entities: []
topics: [Godot, MCP, 2D-assets]
confidence: high
---

# Godot AI MCP 正式资产导入预览要点

## 操作规则

- 当 Godot 工程根目录是仓库的 game 子目录时，MCP 资源路径使用 res://assets/...，不是 res://game/assets/...。
- 先调用 filesystem_manage(reimport)，断言 not_found_count=0 与 reimported_count 一致。
- 临时场景运行后检查 helper_live=true 与 game_capture_ready=true，再抓取 editor_screenshot(source=game)。
- 新导入纹理首帧可能未出现；停止并用同一场景重启后再截图。只有最终同屏截图出现全部目标资源时才通过。
- 临时预览场景完成后删除，确认编辑器恢复到原业务场景。

## 证据

C8-T3 的 v38 普通怪与 v18 玩家同屏导入预览已按此流程通过。图像生成凭据、任务 ID 与原始响应不记录在知识库。