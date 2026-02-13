# 🚀 30 分钟快速开始指南

> 目标：30 分钟内完成一次可验证的 `riscv64` 启动，并进入你自己的构建路径。
>
> ⚠️ **代码示例声明**：本文所有命令与脚本片段均为实例，仅用于演示思路与流程。请按你的系统环境、工具链版本与权限设置进行调整后再执行。
>
> ✍️ **文档署名**：本文档由 **Avrova Donz** 借助 **ChatGPT 5.3** 完成。

---

## 🎯 这份快速指南的定位

这份文档强调两件事：

1. **先成功一次**：先跑通一个已经验证的 `glibc` 案例，快速建立信心。
2. **再自己构建**：马上切换到你自己的 LFS/CLFS 构建流程。

`openRuyi` 最终技术栈是 **glibc + RPM + RISC-V**，所以本指南默认优先带你走 **glibc 方向**。

---

## ⏱️ 30 分钟任务清单

- [ ] 安装宿主机依赖（5-10 分钟）
- [ ] 启动一个可运行的 `glibc` 案例 rootfs（10-15 分钟）
- [ ] 在 guest 内验证 `riscv64`（2 分钟）
- [ ] 初始化你自己的 `CLFS` 工作目录（3 分钟）

完成后，你就具备继续读 `ROADMAP.md` 和 `readme.md` 进行正式构建的上下文。

---

## 1. 宿主机准备（x86_64 Linux）

### 最低要求

- 架构：`x86_64`
- 内存：`>= 4GB`（推荐 `8GB`）
- 磁盘：`>= 20GB` 空闲
- 网络：可访问 GitHub 与上游源码站点

### 安装依赖

Ubuntu / Debian:
```bash
sudo apt update
sudo apt install -y build-essential bison flex bc libncurses5-dev \
    libncursesw5-dev zlib1g-dev libssl-dev wget curl git zstd cpio \
    qemu-user-static qemu-system-misc
```

Fedora:
```bash
sudo dnf groupinstall -y "Development Tools"
sudo dnf install -y bison flex bc ncurses-devel openssl-devel zlib-devel \
    wget curl git zstd cpio qemu-user-static qemu-system-riscv
```

---

## 2. 直接跑通一个 glibc 案例（推荐）

这里使用仓库里已有的 `purofle` 提交（`glibc + systemd`），目的是快速验证运行链路。

### 下载镜像资产

```bash
mkdir -p ~/ruyi-quickstart && cd ~/ruyi-quickstart

wget https://github.com/purofle/riscv-lfs/releases/download/r12.4-45-systemd/rootfs-riscv64-lfs-purofle.tar.zst
wget https://github.com/purofle/riscv-lfs/releases/download/r12.4-45-systemd/Image
```

### 制作磁盘镜像并写入 rootfs

```bash
tar --zstd -xf rootfs-riscv64-lfs-purofle.tar.zst

qemu-img create lfs-riscv64.img 5G
mkfs.ext4 lfs-riscv64.img

sudo mkdir -p /mnt/lfs
sudo mount -o loop lfs-riscv64.img /mnt/lfs
sudo cp -a rootfs-riscv64-lfs-purofle/. /mnt/lfs/
sudo umount /mnt/lfs
```

### 启动 QEMU

```bash
qemu-system-riscv64 \
  -machine virt \
  -m 1G \
  -bios default \
  -kernel Image \
  -append "root=/dev/vda rw console=ttyS0" \
  -drive file=lfs-riscv64.img,format=raw,id=hd0,if=none \
  -device virtio-blk-device,drive=hd0 \
  -nographic
```

根据该提交文档，默认登录信息为：
- 用户名：`root`
- 密码：`114514`

### 启动后验证

进入系统后执行：

```bash
uname -m
getconf GNU_LIBC_VERSION
```

预期：
- `uname -m` 输出 `riscv64`
- `getconf GNU_LIBC_VERSION` 输出 `glibc x.y`

如果能看到这两项，你的基础环境已经打通。

---

## 3. 初始化你自己的构建目录（马上开始）

你已经跑通了案例，下面马上切入自己的构建环境：

```bash
export CLFS=$HOME/clfs-riscv64
export TARGET=riscv64-unknown-linux-gnu

mkdir -p ${CLFS}/{cross-tools,tools,sources}
mkdir -p ${CLFS}/{bin,boot,dev,etc,home,lib,lib64,mnt,opt,proc,root,run,sbin,srv,sys,tmp,usr,var}
```

建议把这两个环境变量写入 `~/.bashrc`，避免后续遗漏。

---

## 4. 下一步怎么走

### 推荐顺序

1. 看 `ROADMAP.md`：选路线（建议优先 `glibc` 路线）
2. 看 `readme.md`：按三阶段方法（Pass1/Pass2/Pass3）正式构建
3. 卡住就查 `FAQ.md`
4. 准备提交前跑 `CHECKLIST.md`

### 关于 musl 路线

musl 路线可以作为入门体验，但如果你的目标是后续参与 openRuyi，建议尽快切回 `glibc` 路线，优先积累 `glibc + RPM` 相关经验。

---

## ✅ 完成标记

满足以下 4 项即可判定你已经完成快速开始：

- [ ] QEMU 成功启动到 shell 或登录提示
- [ ] `uname -m` 为 `riscv64`
- [ ] `getconf GNU_LIBC_VERSION` 可正常输出
- [ ] 已创建自己的 `${CLFS}` 工作目录

---

## 💬 一句话建议

先把“可运行”拿下，再把“可复现”拿下，最后再追求“可维护”。这就是从新手村走向 openRuyi 开发的正确节奏。
