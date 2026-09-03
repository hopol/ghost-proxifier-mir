<div align="center">

# ghost-proxifier-mir

ghost-proxifier-pro 上游仓库的源码与 Release 文件镜像

[![Upstream](https://img.shields.io/badge/upstream-liliBestCoder%2Fghost--proxifier--pro-181717?logo=github&logoColor=white)](https://github.com/liliBestCoder/ghost-proxifier-pro)
[![Branch](https://img.shields.io/badge/branch-master-2ea44f?logo=git&logoColor=white)](https://github.com/liliBestCoder/ghost-proxifier-pro/tree/master)
[![Sync](https://img.shields.io/github/actions/workflow/status/hopol/ghost-proxifier-mir/sync.yml?label=sync&logo=githubactions&logoColor=white)](https://github.com/hopol/ghost-proxifier-mir/actions/workflows/sync.yml)
[![Release](https://img.shields.io/github/actions/workflow/status/hopol/ghost-proxifier-mir/release.yml?label=release&logo=githubactions&logoColor=white)](https://github.com/hopol/ghost-proxifier-mir/actions/workflows/release.yml)
[![Mirror License](https://img.shields.io/badge/mirror-MIT-blue.svg)](LICENSE)

[上游仓库](https://github.com/liliBestCoder/ghost-proxifier-pro) · [镜像 Releases](https://github.com/hopol/ghost-proxifier-mir/releases) · [Actions](https://github.com/hopol/ghost-proxifier-mir/actions)

</div>

---

## 📌 说明

本仓库用于镜像 [`liliBestCoder/ghost-proxifier-pro`](https://github.com/liliBestCoder/ghost-proxifier-pro) 的源码和 GitHub Release 文件。

- 源码来自上游 `master` 分支，导出到 `upstream/`。
- Release 文件来自上游最新 GitHub Release，并重新发布到本仓库的 Releases。
- 本仓库不修改上游源码，不提供上游项目的官方支持。

> [!NOTE]
> 上游项目描述：Ghost Proxifier Pro 是一款专为 Windows 平台打造的高性能进程级透明代理引擎，旨在解决现代复杂网络环境下的两大痛点：「特定应用不走系统代理」以及「多重 VPN/代理路由冲突」。  核心亮点描述： 真正的进程级细粒度控制：不同于传统的全局代理或 TUN 模式，它基于 MinHook 直接拦截应用层 Winsock API。这意味着您可以精确指定某个进程（如 Chrome、游戏或特定工具）走代理，而无需修改系统全局路由表。 「全家桶」自动关联技术：Pro 版独有的进程追踪算法。只需接管母进程，它便能智能识别并自动拦截该软件启动的所有关联子进程，彻底告别繁琐的手动配置。 零侵入、高兼容：采用独创的 Lazy Handshake (延迟握手) 机制，完美兼容现代异步 IO。功能说明、安装方式、更新内容和使用要求请以上游仓库为准。

## 📁 镜像范围

| 内容 | 位置 | 说明 |
|---|---|---|
| 上游源码 | `upstream/` | 通过 `git archive` 从上游 `master` 分支导出。 |
| 同步信息 | `upstream/.sync-info` | 记录上游提交、同步时间、分支和版本或来源引用。 |
| 源码标签 | `mirror-source-…` | 对应一次源码同步。 |
| Release 文件 | 本仓库 Releases | 下载自上游 GitHub Release。 |
| Release 标签 | `mirror-release-{上游标签}` | 对应一个上游 Release。 |

## 🔄 自动同步

```mermaid
flowchart LR
    A["上游仓库<br>liliBestCoder/ghost-proxifier-pro"] --> B["sync.yml<br>检查 master 分支"]
    B --> C{"上游提交是否变化"}
    C -->|"否"| D["结束"]
    C -->|"是"| E["导出源码到 upstream/"]
    E --> F["写入 .sync-info"]
    F --> G["提交并创建源码标签"]

    A --> H["release.yml<br>检查最新 Release"]
    H --> I{"本仓库是否已镜像"}
    I -->|"是"| J["结束"]
    I -->|"否"| K["下载附件或跳过空附件"]
    K --> L["创建镜像 Release"]
```

> [!IMPORTANT]
> GitHub Actions 中的定时任务使用 UTC 时间。cron 表达式的日期字段为 `*/5`，通常在每月 1、6、11、16、21、26、31 日运行，并不等同于严格每 5 天运行一次。

## 🧾 同步信息

```ini
commit=0123456789abcdef...
timestamp=2026-08-07T00:00:00Z
upstream_url=https://github.com/liliBestCoder/ghost-proxifier-pro
upstream_branch=master
source_ref=0123456
```

`source_ref`：上游没有可可靠读取的版本文件，使用 `git describe --tags` 的来源引用。

同步脚本会在删除 `upstream/` 前读取已提交的 `.sync-info`。只有上游提交变化时，才会更新源码、创建提交和标签。

## 💻 本地同步源码

`sync.sh` 用于本地手动同步源码。它需要 Git、Bash 环境（Linux、macOS、WSL 或 Git Bash）和对镜像仓库的推送权限。

```bash
git clone https://github.com/hopol/ghost-proxifier-mir.git
cd ghost-proxifier-mir
chmod +x sync.sh
./sync.sh
```

## 🚀 Release 镜像

`release.yml` 会读取上游最新 Release。若本仓库不存在对应镜像标签，则会下载上游附件并创建镜像 Release。上游 Release 没有附件时，工作流会跳过下载，并创建仅含说明的镜像 Release。

> [!WARNING]
> 本仓库不会校验、重签名或重新打包上游附件。下载和使用前请自行确认来源、版本和文件完整性。

## 🛠️ 维护常用命令

```bash
# 查看当前镜像对应的上游提交
git show HEAD:upstream/.sync-info

# 列出镜像标签
git tag -l 'mirror-*'

# 手动拉取上游分支
git fetch upstream master --tags
```

## ⚖️ 许可证

- 本仓库的同步脚本、GitHub Actions 工作流和文档采用 [MIT License](LICENSE)。
- `upstream/` 中的内容受上游许可证约束。GitHub API 报告的上游许可证为：`NOASSERTION`。

---

<div align="center">

本仓库只是镜像，不是上游项目官方仓库。

[返回顶部](#ghost-proxifier-mir)

</div>
