# CLAUDE.md

本文件为 Claude Code (claude.ai/code) 在本仓库中工作时提供指导。

## 构建

```bash
CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build
```

## 测试

```bash
go test ./...                    # 运行所有测试
go test ./server/...             # 运行 server 包测试
go test -run TestXXX ./firewall/ # 运行单个测试
```

## 架构

SecGroupFE 是安全组 HTTP API 服务，是 VPC 云平台的"前端"层，负责 HTTP 请求解析、参数校验、业务逻辑编排，持久化委托给后端 gRPC SecGroup 服务。

### 请求流转

1. HTTP 请求到达 `uhttp.RouteHTTP`（根路径 `/`）
2. 根据 `Action` 参数路由到对应 handler（通过 `uftask.RegisterHTTPTaskHandle` 注册）
3. `server/` 包中的 handler 解析输入（`uhttp.InputRequest` 基于结构体 tag 校验），编排业务逻辑，调用 gRPC/HTTP 下游服务
4. 通过 `uhttp.OutputResponse` 返回响应

### 核心包

- **server/** — HTTP handler 和核心业务逻辑。所有 API 在 `server.go:register()` 中注册。
- **service/** — 下游服务客户端（gRPC 拨号 + 重试）。每个子包封装一个后端服务：
  - `secgroup/` — 安全组后端 gRPC 客户端（持久化）

### 错误处理模式

错误码定义在 `uhttp/response_code.go`（范围 208601-208839）。使用 `GenerateErrorCode(code, internalErr, params...)` 返回 `map[string]interface{}`，包含 `RetCode`、`Message` 和 `internalErr`（布尔值，用于监控区分内外部错误）。

### 输入校验

请求结构体使用 struct tag：`json`、`required:"true"`、`default:"value"`。由 `uhttp.InputRequest()` 通过反射执行校验。部分字段会自动去除空格（在 `blankSpaceType` map 中配置）。

### API 命名约定

- 公开 API：`CreateSecGroup`、`DeleteSecGroup`、`UpdateSecGroup`、`DescribeSecGroup` 等
- 内部 API（`I` 前缀）：`IGetSecGroup`、`IAssociateSecGroup` 等 — 不需要账号信息，供内部服务调用
- SRE API（`SRE` 前缀）：运维/管理工具接口，用于排查和数据清理
