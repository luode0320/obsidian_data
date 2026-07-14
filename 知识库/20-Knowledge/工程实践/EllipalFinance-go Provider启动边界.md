# EllipalFinance-go Provider 启动边界

- 状态: active
- 更新时间: 2026-07-13
- 项目: [[EllipalFinance-go]]

## 决策

`app/exchange/provider/init.go` 只负责按固定顺序平铺调用 `New().Start(true)`，不为单个服务商增加条件分支或专属日志。

## 落地模式

服务商专属的启用开关、配置校验、失败日志和不注册短路，内聚到服务商自身的 `Start` 包装边界。通过嵌入 `interfaces.IExchangeBase` 仅覆写 `Start`，再委托公共 `ProviderBase.Start`，无需修改公共接口或公共 Provider 基类。

## 已验证

- 默认关闭配置可安全读取。
- 本地 Mock、全仓 go test、go build 和 NEAR Provider 定向 vet 通过。
- 不记录任何密钥、令牌或环境配置值。

## 2026-07-13 文件职责归位

- `api.go` 聚焦范围、汇率、下单、状态及上游 Quote/status/submit 调用。
- `remoteTask.go` 聚焦兑出/兑入列表组装。
- `redisTask.go` 聚焦 Token 数据源。
- `util.go` 聚焦通用 HTTP Client、请求封装和转换工具。
- 该归位遵循 `doc/新增服务商.md` 与 NEXCHANGE 现有布局，未改变公开符号和业务行为。