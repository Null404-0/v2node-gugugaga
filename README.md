# v2node
A v2board backend base on moddified xray-core.
一个基于修改版xray内核的V2board节点服务端。

> 本仓库为 [wyx2685/v2node](https://github.com/wyx2685/v2node) 的修改版。安装脚本 / 管理脚本 / 节点二进制 release 均以本仓库 **Null404-0/v2node-gugugaga** 为准。

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

### 2026-11-23
- 同步上游至 [`5c2360f`](https://github.com/wyx2685/v2node/commit/5c2360f95db39f61271e701eabdee200ba8075ea)，包含 Xray-core v26.10.31、SS2022 密钥长度和 Freedom 出站域名策略修复。
- Alpine 升级时会重启到新版本。`v2node log` 显示最近 200 行并实时跟踪，每 15 分钟检查一次日志，超过 10 MiB 时轮转并保留一份压缩备份。

### 2026-06-17
- Alpine 支持本地日志查看。
- `v2node log` 支持实时跟踪。
- 启动重启自动清空旧日志。
- 日志超 10M 自动轮转压缩。
- 自更新脚本固定回拉本仓库。
- 安装包下载改用本仓库。
- 独立安装包支持三架构。
