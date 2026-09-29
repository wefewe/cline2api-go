# wefewe/cline2api-go

`cline2api` 的自建构建镜像仓库（用于本集群内部部署，非上游官方仓库）。

## 这是什么

- **上游**：`github.com/luawei1/cline2api`（Go 版，MIT，多账号轮询 + OpenAI/Anthropic 双协议 + 管理后台 + 出口代理池）。
- 仓库结构：
  - `upstream/` —— 上游源码快照（自动同步自 luawei1/cline2api 最新 Release tag）；
  - `.github/workflows/build.yml` —— 多架构（amd64/arm64）构建并推送 `ghcr.io/wefewe/cline2api-go`；
  - `.github/workflows/sync-upstream.yml` —— 每日自动拉取上游最新 tag 并提交（有变化即触发重新构建）。

> 为什么单独建仓：本集群原 `ghcr.io/wefewe/cline2api` 基于 `bouderer/cline2api`（TypeScript 版）。
> 现迁移到更活跃、功能更全的 luawei1 Go 版，为保持镜像血缘清晰与可回滚，另立本仓库。

## 许可

上游为 MIT 许可，版权归原作者，详见 `upstream/LICENSE`。

## 部署（本集群）

- 镜像：`ghcr.io/wefewe/cline2api-go:latest`
- 容器监听：`0.0.0.0:3457`（宿主机映射由 Swarm 栈负责）
- 数据：`/app/.cline-accounts.json`（账号池）、`/app/.cline-config.json`（配置）
  —— 由宿主机文件直接挂载（程序按可执行文件目录 `/app` 查找数据文件）
- 编排见 `/opt/swarm/stacks/us/cline2api.yml`
