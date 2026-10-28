# Hub-Mirror 引擎（自持版）

本目录是 Hub-Mirror MCP Server 内置的镜像搬运引擎，**已从上游收编为自有代码**。

## 来源

- 上游项目：[togettoyou/hub-mirror](https://github.com/togettoyou/hub-mirror)（MIT License，详见 [LICENSE](./LICENSE)）
- 收编 commit：`0a3cd37`（2025-02-25 上游最新 HEAD）
- 上游项目自 2024-01 起核心代码已基本停更，为保证可复现性与可控性，将其收编进本项目

## 工作方式

1. MCP Server 启动/提交任务时，调用 `syncEngine()` 将本目录代码同步到代理仓库（`GITHUB_PROXY_REPO`）根目录
2. 同步通过仓库根目录的 `engine-commit.txt` 版本标记做增量判断：版本一致则跳过
3. 代理仓库的 `mirror.yml` Workflow checkout 自身仓库后直接 `go run main.go`，**不再依赖任何外部仓库**

## 版本管理

- 修改本目录代码后，**必须**同步递增 [src/infrastructure.ts](../src/infrastructure.ts) 中的 `ENGINE_VERSION` 常量，否则修改不会推送到远端
