---
title: NEAR Intents USD 最小范围探测
type: decision
status: confirmed
project: EllipalFinance-go
date: 2026-07-13
tags:
  - NEAR_INTENTS
  - range-probe
  - 1Click
---

# NEAR Intents USD 最小范围探测

- 决策：NEAR Intents 不再返回开放最小范围；并发发送 0.01、0.1、0.5 USD 的 INTENTS dry EXACT_INPUT Quote，返回最小成功档位。
- 回退：三档均失败时，以 200 USD 按 origin Token 的 Price 和 Decimals 换算为保守最小金额。
- 地址：范围探测不依赖用户链地址；配置的 fee recipient 同时作为 refundTo 与 recipient，三个地址 type 都为 INTENTS。
- 精度：金额使用 decimal 计算并截断到 Token Decimals，随后转最小单位；Quote 的 amountIn 与 amountOut 均须为正才视为成功。
- 兼容：公共缓存新增自定义 decimal USD 档位入口；SwapKit 保留 1/5/50 USD 探测和 200 USD 回退语义。
- 验证：仅在 WSL local 和 httptest.Server 执行 Mock、go vet、go test、go build；未调用真实上游，也未记录凭证。

关联：[[EllipalFinance-go]]


Verification: bridge create/read verified on 2026-07-13.
