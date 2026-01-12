# RISC-V Rootfs 试炼：LFS 门槛

Guten Tag! 旅人，你已踏入门槛之门。

你的任务，是锻造一份 **riscv64** 的 **Linux From Scratch** rootfs，并将它封存在你自己的 GitHub **Releases** 宝库中。

本仓库不收 rootfs 本体。我们只收两样东西:
1) 你自己编写的包含"如何运行"的 Markdown 格式心得卷轴
2) 一张在该环境内运行 `fastfetch` 的截图

若你的 PR 被合并，城堡机关 (CI) 将使用你的 **GitHub ID** 为你在内部王国创建账号。

---

## 你要交付什么

你的 PR 必须包含:

### 1) 心得卷轴

新建文件:

- `submissions/<githubid>.md`

### 2) fastfetch 截图

提交一张 png 截图到仓库:

- `evidence/<githubid>/fastfetch.png`

截图要求:

- 必须是在你发布的 riscv64 rootfs 环境里运行 `fastfetch` 得到的输出
- 截图中需能看到架构信息 (riscv64) 与基本系统信息

> **不要**把 rootfs (或任何大二进制) 提交到本仓库。请只提交 Markdown + 截图。

---

## 你的 rootfs 应该放在哪里

rootfs 必须放在你自己的任意 GitHub Repo 的 Releases 中，并满足以下要求:

- Releases 文件名**必须严格为**:
  - `rootfs-riscv64-lfs-<githubid>.tar.zst`

你需要在卷轴里提供:

- rootfs Release 页面链接
- rootfs 的 sha256

---

## 禁忌

- **不要**在 rootfs 内放任何密钥、Token、SSH Key、`.git-credentials`、私钥文件、云服务配置等。
- **不要**把你机器上的真实 `/etc/ssh/`、`~/.ssh/`、`~/.config/` 之类目录不小心打包进去。

你在卷轴中需要做出声明 (模板里已写)。

---

## 卷轴模板

将下列内容复制到 `submissions/<githubid>.md` 并填写：

```md
# <githubid> 的试炼记录

## 基本信息

- GitHub ID: <githubid>
- 联系邮箱: <email>
- rootfs 发布 Repo: https://github.com/<you>/<repo>

## Rootfs 资产

- 文件名: rootfs-riscv64-lfs-<githubid>.tar.zst
- SHA256: <sha256>

## 如何从 rootfs 运行起来

> 目标：从"下载 rootfs"到"进入环境并跑起 fastfetch"的最短步骤。

### 方式 1

1. ...

## fastfetch 证据

<!-- 此处插入你的 fastfetch 截图 -->

## 这是如何锻造的 (LFS 过程简述)

- 参考的教程/版本:
- 关键配置（toolchain / glibc / 内核 / init / busybox / 包策略等）:
- 与"原教旨 LFS"的偏离（如有）:

## 你踩过的坑

- 坑 1: ...
- 坑 2: ...

## 已知问题 / TODO (如有)

- ...

## 许可与安全声明

- 我确认 rootfs 不包含任何密钥/Token/SSH Key/凭据/私人数据。
```
