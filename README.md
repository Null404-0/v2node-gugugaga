# v2node
A v2board backend base on moddified xray-core.
一个基于修改版xray内核的V2board节点服务端。

> 本仓库为 [wyx2685/v2node](https://github.com/wyx2685/v2node) 的修改版。安装脚本 / 管理脚本以本仓库 **Null404-0/v2node-gugugaga** 为准；节点二进制仍从上游 release 下载（本仓库暂未发布独立 release）。

**注意： 本项目需要搭配[修改版V2board](https://github.com/wyx2685/v2board)**

## 软件安装

### 一键安装

```
wget -N https://raw.githubusercontent.com/Null404-0/v2node-gugugaga/main/script/install.sh && bash install.sh
```

## 构建
``` bash
GOEXPERIMENT=jsonv2 go build -v -o build_assets/v2node -trimpath -ldflags "-X 'github.com/wyx2685/v2node/cmd.version=$version' -s -w -buildid="
```

## 更新日志

### 2026-06-17
- **修复 Alpine 系统无法查看日志**：OpenRC 启动脚本(`/etc/init.d/v2node`)增加 `output_log`/`error_log`，将程序 stdout/stderr 落盘到 `/var/log/v2node.log`（Alpine 无 journald，原先日志被直接丢弃）；`v2node log` 在 Alpine 下改用 `tail -f` 实时查看，替换原“暂不支持”提示。
- **日志体积治理（防止日志越滚越大）**：
  - 每次启动 / 重启自动清空日志（启动脚本 `start_pre` 截断），`v2node log` 只显示**当前会话（本次运行）**的日志。
  - 通过 `logrotate` 将单文件限制在 **10M 以内**（busybox crond 每 15 分钟检查一次，超限即轮转 + 压缩，仅保留 1 个备份）。
- 安装 / 管理脚本的**自更新地址改为指向本仓库**（`Null404-0/v2node-gugugaga` 的 `main` 分支），避免 `v2node update` 时被上游脚本覆盖；节点二进制仍从上游 `wyx2685/v2node` 的 release 下载。
- 老用户需执行一次 `v2node update` 重写启动脚本并重启后，日志相关功能方可生效。
- **GitHub Actions 精简（省额度，无定时任务）**：删除定时运行的 CodeQL 扫描；二进制构建（`release.yml`）与 Docker 镜像工作流均改为**仅在「发布 Release」或手动触发时运行**；二进制构建架构由 25+ 精简为 3 个常用 Linux 架构（`amd64` / `arm64` / `s390x`）。发布一个 Release 即自动产出二进制（含 geoip/geosite）与多架构 Docker 镜像。
