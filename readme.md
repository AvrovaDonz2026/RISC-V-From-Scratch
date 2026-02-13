# 🎉 欢迎来到 openRuyi 新手村！

> 🏰 **恭喜你迈出了成为系统工程师的第一步！**
> 
> 这里是通往 **openRuyi 操作系统** 的第一道门槛，也是一段精彩技术旅程的起点。
>
> ⚠️ **代码示例声明**：本文所有命令与脚本片段均为实例，仅用于演示思路与流程。请根据你的发行版、权限模型、目录布局和版本差异自行调整，并先在隔离环境验证。
>
> ✍️ **文档署名**：本文档由 **Avrova Donz** 借助 **ChatGPT 5.3** 完成。

---

## 🌟 你即将收获什么

**openRuyi** 是一个基于 **glibc + RPM**、运行在 **RISC-V** 架构上的 Linux 发行版。通过这里的试炼，你将：

- 🛠️ **亲手从零构建**一个可运行的 riscv64 Linux 系统
- 🧠 深入理解 Linux 系统的基本构成
- 🤝 加入志同道合的技术社区
- 🚀 为参与 openRuyi 开发打下坚实基础

**无需担心难度** —— 无论选择哪条路线，我们都为你准备了详细的指南和丰富的参考资料。相信自己，你一定可以！

### 💡 关于技术路线

openRuyi 使用 **glibc + RPM** 技术栈。我们**建议**优先尝试基于 glibc 的路线（📚 经典路线 或 🏭 完整路线），这些经验对未来的开发工作更有帮助。  
如果你的目标是进入 openRuyi 正式开发，默认应把 **glibc 路线作为主线任务**。

但请记住，**选择权完全在你**：
- 想快速体验？🚀 极速路线欢迎你！
- 想深入钻研？📚 经典路线等你挑战！
- 追求完美？🏭 完整路线让你大展身手！

**无论哪条路线，完成试炼都能获得认可！** ✨

---

## 📚 新手村地图

```
RISC-V-From-Scratch/  (本仓库)
│
├── 📖 readme.md - 新手村总览与方法论 (你在这里)
├── 🚀 QUICKSTART.md - 快速开始指南 (30分钟上手)
├── 🗺️ ROADMAP.md - 技术路线选择指南
├── ❓ FAQ.md - 常见问题与解决方案
├── ✅ CHECKLIST.md - 通关检查清单
├── 📋 README.md / README.zh-CN.md - 提交规范
├── 📂 submissions/ - 试炼者提交的卷轴 (参考案例)
├── 📂 evidence/ - 运行截图证据
└── 📎 AGENTS.md - 维护者指南
```

### 推荐阅读顺序

```
第一次来？
    │
    ▼
┌─────────────────────────────────┐
│ 🗺️ ROADMAP.md                   │ ◄── **建议先读！** 选路线 + 看案例
│   明确：openRuyi 使用 glibc     │
└────────────────┬────────────────┘
                 │
        ┌────────┴────────┐
        │                 │
        ▼                 ▼
┌─────────────────┐ ┌─────────────────┐
│ 📚 经典路线      │ │ 🏭 完整路线      │
│   (推荐)        │ │   (进阶)        │
│ glibc+SysVinit  │ │ glibc+systemd   │
└────────┬────────┘ └────────┬────────┘
         │                   │
         └─────────┬─────────┘
                   │
    ┌──────────────┼──────────────┐
    │              │              │
    ▼              ▼              ▼
┌────────┐  ┌─────────────────┐  ┌────────┐
│🚀 QUICK│  │📂 submissions/  │  │📖本页  │
│START   │  │  查看他人提交   │  │方法论  │
│(可选)  │  │  获取灵感       │  │       │
└────────┘  └─────────────────┘  └────────┘
         │              │              │
         └──────────────┼──────────────┘
                        │
                        ▼
               ┌─────────────────┐
               │ ❓ FAQ          │
               │   常见问题      │
               └────────┬────────┘
                        │
                        ▼
               ┌─────────────────┐
               │ ✅ CHECKLIST    │
               │   提交前检查    │
               └─────────────────┘
```

### ⚡ 快速开始

| 你的状态 | 推荐行动 |
|----------|----------|
| 🆕 刚开始，不知道从何入手 | 先看 **🗺️ ROADMAP.md**，选择适合你的路线！ |
| 🚀 迫不及待想动手 | 直接跳到 **🚀 QUICKSTART.md**，30分钟见成果！ |
| 🤔 遇到问题卡住了 | 别急，**❓ FAQ.md** 里有答案！ |
| ✨ 准备提交成果 | 对照 **✅ CHECKLIST.md**，确保万无一失！ |

---

## 🎮 试炼流程

### 第一阶段：准备 (预计 1-2 天)

1. **选择路线**（建议先读 [🗺️ ROADMAP.md](ROADMAP.md)）：
   
   **我们推荐的路线（与 openRuyi 技术栈一致）：**
   - 📚 **经典路线**：glibc + SysVinit（2-3天）— **适合大多数同学**
   - 🏭 **完整路线**：glibc + systemd（3-5天）— **追求完整体验**
   
   **其他选择：**
   - 🚀 **极速路线**：musl + BusyBox（20分钟）— **快速入门**
   
   > 💡 **选择权在你** —— 无论哪条路线，完成试炼都能获得认可。

2. **了解目标**：阅读本文档，理解 LFS/CLFS 的基本概念

3. **搭建环境**：准备一台 x86_64 Linux 宿主机（虚拟机即可）

4. **学习前辈经验**：浏览 `submissions/` 目录，查看其他同学的提交，获取灵感和参考

### 第二阶段：构建 (预计 1-2 周)

按照下面的**三段式构建方法论**，亲手打造你的 riscv64 根文件系统：

| 阶段 | 任务 | 输出 | 环境 |
|------|------|------|------|
| **Stage 1** | 构建交叉编译器 | `riscv64-unknown-linux-gnu-gcc` | Host (x86_64) |
| **Stage 2** | 构建临时系统 | `${CLFS}/tools` 完整工具链 | Host + QEMU |
| **Stage 3** | 构建最终系统 | 可启动的 rootfs | Chroot / QEMU |

### 第三阶段：提交 (预计 1 天)

1. 将 rootfs 发布到你自己的 GitHub Releases
2. 按照模板撰写提交文档
3. 提交 PR 到本仓库

---

## 🏗️ 三段式构建核心方法论

### 1. 三元目录隔离（铁律）

任何偏离此结构的构建都将在后期引入路径污染与权限混乱：

```bash
${CLFS}/              # 最终目标系统根目录（riscv64 架构）
${CLFS}/cross-tools/  # Host 架构可执行文件（x86_64→riscv64 交叉编译器）
${CLFS}/tools/        # Target 架构可执行文件（riscv64，构建期间通过 QEMU 解释执行）
${CLFS}/sources/      # 源码与构建目录（必须在普通用户权限下操作）
```

**关键原则**：
- `cross-tools` 与 `tools` **绝对分离**
- 所有构建过程不得在 `${CLFS}` 外安装任何文件

### 2. 版本锁定矩阵（关键）

RISC-V 生态演进快速，必须预先验证以下兼容性：

| 组件 | 推荐版本 | 注意事项 |
|------|----------|----------|
| QEMU | ≥ 8.0 | 低版本无法正确处理 glibc 2.38+ 的 `clone3` |
| GCC | 13.x | 与 Binutils 版本需匹配 |
| Binutils | 2.42 | - |
| Linux Kernel | 6.6.x | 头文件版本不必与运行内核完全一致 |

### 3. 编译器构建三 passes（CLFS 正统）

**Pass 1: 最小化交叉编译器**（无 libc）
```bash
# Binutils（无依赖）
../binutils-2.42/configure \
  --prefix=${CLFS}/cross-tools \
  --target=${TARGET} \
  --with-sysroot=${CLFS} \
  --disable-nls --disable-werror

# GCC（仅 C 语言，无 libc）
../gcc-13.2.0/configure \
  --prefix=${CLFS}/cross-tools \
  --target=${TARGET} \
  --with-sysroot=${CLFS} \
  --enable-languages=c \
  --disable-shared \
  --disable-threads \
  --without-headers
```

**Pass 2: C 库安装**
- **glibc 路径**：先安装内核头文件，再构建 glibc，使用 Pass 1 GCC
- **musl 路径**：直接配置，无需预先安装内核头文件

**Pass 3: 完整交叉编译器**
重新构建 GCC，启用 `--enable-threads=posix` 与 `--enable-shared`

---

## 🚀 进入目标环境

### 方案 A：QEMU 用户态 + Chroot（推荐）

```bash
# 注册 QEMU 二进制解释器（需 root，一次性）
cp /usr/bin/qemu-riscv64-static ${CLFS}/usr/bin/

# Debian/Ubuntu（推荐）
sudo update-binfmts --enable qemu-riscv64

# systemd 发行版（可选）
# sudo systemctl restart systemd-binfmt

# Chroot 进入（需 root）
sudo chroot "${CLFS}" /usr/bin/env -i \
  HOME=/root TERM=${TERM} PS1='(riscv64) \u:\w\$ ' \
  PATH=/tools/bin:/bin:/usr/bin \
  /tools/bin/bash --login
```

### 方案 B：Initramfs + QEMU 系统模式

当 QEMU 用户态不可行时，采用两阶段启动：

```bash
# 打包 cpio
find . | cpio -o -H newc | zstd > ../initramfs.cpio.zst

# 系统模式启动
qemu-system-riscv64 \
  -machine virt \
  -m 512M \
  -bios default \
  -kernel Image \
  -initrd initramfs.cpio.zst \
  -append "init=/bin/sh" \
  -nographic
```

---

## ⚠️ RISC-V 架构特化要点

### 页大小

RISC-V 支持 4KB/16KB/64KB 页大小，但用户态软件常硬编码 4KB 假设：

```bash
# 强制使用 4KB 页
# 内核配置: CONFIG_PAGE_SIZE_4KB=y
# glibc 构建: libc_cv_pagesize=4096
```

### 启动流程

```
OpenSBI (M-mode) → Linux Kernel (S-mode) → init (U-mode)
```

**无需构建 EDK2/UEFI**，QEMU `-bios default` 已提供 OpenSBI。

### musl 动态链接器陷阱

musl 的动态链接器默认使用绝对路径 `/lib/libc.so`，在 chroot 中会导致 "file not found"：

```bash
# 修复：使用相对链接
cd ${CLFS}/lib
ln -s libc.so ld-musl-riscv64.so.1
```

### 压缩指令集（RVC）

确保工具链与内核均启用 RVC 支持（默认）：
- GCC: `--with-arch=rv64gc`
- 内核: `CONFIG_RISCV_ISA_C=y`

---

## 🎨 Init 系统选择

| 方案 | 复杂度 | 适用场景 | 建议 |
|------|--------|----------|------|
| **BusyBox init** | ⭐ 低 | 教学验证、嵌入式 | 🥇 首次构建首选 |
| **SysVinit** | ⭐⭐ 中 | 复古系统 | 进阶尝试 |
| **systemd** | ⭐⭐⭐ 高 | 现代桌面/服务器 | 熟练后再挑战 |

**决策建议**：首次构建务必选择 BusyBox 静态链接方案，验证完整启动流程后再引入复杂度。

---

## ✅ 阶段性验证节点

### Node 1：工具链完整性
```bash
echo '#include <stdio.h>
int main(void) { printf("Hello %s\\n", "RISC-V"); return 0; }' | \
  ${TARGET}-gcc -x c - -o /tmp/test -static && \
  file /tmp/test  # 应显示静态链接的 riscv64 可执行文件
```

### Node 2：Chroot 环境
```bash
chroot "${CLFS}" /tools/bin/bash -c "gcc --version"
```

### Node 3：系统启动
```bash
qemu-system-riscv64 ... -append "init=/bin/sh"  # 单用户模式验证
```

### Node 4：服务管理
验证 `reboot` 与 `poweroff` 能正确触发内核关机。

---

## 🐛 常见故障速查

更多问题请查阅 [❓ FAQ.md](FAQ.md)，包含 20+ 个问题的详细解答。

| 故障 | 根因 | 修复 |
|------|------|------|
| **VFS: unable to mount root** | 缺少 `CONFIG_VIRTIO_BLK=y` | 检查 `dmesg \| grep virtio` |
| **Exec format error** | qemu-riscv64-static 未复制或 binfmt 未注册 | 验证复制和注册步骤 |
| **not found** (文件存在) | 动态链接器路径错误 | `readelf -l \| grep interpreter` |
| **内核启动后崩溃** | 设备树不匹配或缺少串口配置 | 添加 `earlycon=sbi` |

### 常见陷阱速查

| 问题 | 症状 | 解决方案 |
|------|------|----------|
| **musl 动态链接器** | chroot 中报 "not found" | 创建相对符号链接：`ln -s libc.so ld-musl-riscv64.so.1` |
| **BusyBox tc** | 编译报错 `TCA_CBQ_MAX` 未声明 | `.config` 中禁用 `CONFIG_TC` |
| **QEMU + glibc 2.42** | 程序运行崩溃 | 升级 QEMU 或使用打补丁的版本 |
| **chroot 黑屏** | 进入后无提示符 | chroot 前必须先安装 bash！ |
| **util-linux 安装报错** | `chgrp: Operation not permitted` | 检查必要工具是否存在，可忽略此错误 |

---

## 📋 提交规范

### 文件命名

| 文件 | 命名格式 | 示例 |
|------|----------|------|
| 提交卷轴 | `<githubid>.md` | `donz2026.md` |
| rootfs 包 | `rootfs-riscv64-lfs-<githubid>.tar.zst` | `rootfs-riscv64-lfs-donz2026.tar.zst` |
| 截图 | `fastfetch.png` / `neofetch.png` | `fastfetch.png` |

### 卷轴必须包含

1. ✅ 基本信息（GitHub ID、邮箱、Release 链接）
2. ✅ Rootfs SHA256 校验值
3. ✅ "如何运行"详细步骤
4. ✅ LFS 构建过程简述
5. ✅ 踩坑记录
6. ✅ 安全声明

### 🧾 Scroll Template（卷轴模板）

将下面模板复制到 `submissions/<githubid>.md` 后按实际情况填写：

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

> 目标：从“下载 rootfs”到“进入环境并跑起 fastfetch/neofetch”的最短步骤。

### 方式 1

1. ...

<!-- 可选区 -->

## fastfetch / neofetch 证据

<!-- 此处插入截图（可选） -->

## 这是如何锻造的（LFS 过程简述）

- 参考的教程/版本:
- 关键配置（toolchain / glibc / 内核 / init / busybox / 包策略等）:
- 与“原教旨 LFS”的偏离（如有）:

## 你踩过的坑

- 坑 1: ...
- 坑 2: ...

## 已知问题 / TODO（如有）

- ...

## 安全声明

- 我确认 rootfs 不包含任何密钥/Token/SSH Key/凭据/私人数据。
```

### 📜 旧版 README 原文保留区（完整并入）

以下内容来自旧版提交说明，按原意完整保留，便于对照使用。

#### RISC-V Rootfs 试炼：LFS 门槛

Guten Tag! 旅人，你已踏入门槛之门。

你的任务，是锻造一份 **riscv64** 的 **Linux From Scratch** rootfs，并将它封存在你自己的 GitHub **Releases** 宝库中。

本仓库不收 rootfs 本体。我们只收两样东西:
1. 你自己编写的包含"如何运行"的 Markdown 格式心得卷轴
2. 一张在该环境内运行 `fastfetch` 或者 `neofetch` 的截图 (可选)

若你的 PR 被合并，城堡机关将使用你的 **GitHub ID** 为你在内部王国创建账号。

#### 你要交付什么

你的 PR 必须包含心得卷轴，需要新建如下文件:

- `submissions/<githubid>.md`

你的 PR 可以包含 fastfetch 或者 neofetch 的 png 截图，需要新建如下文件:

- `evidence/<githubid>/fastfetch.png`

截图要求:

- 必须是在你发布的 riscv64 rootfs 环境里运行 fastfetch 或者 neofetch 得到的输出
- 截图中需能看到架构信息 (riscv64) 与基本系统信息

> **不要**把 rootfs (或任何大二进制) 提交到本仓库。请只提交 Markdown (+ 截图)。

#### 你的 rootfs 应该放在哪里

rootfs 必须放在你自己的任意 GitHub Repo 的 Releases 中，并满足以下要求:

- Releases 文件名**必须严格为**:
  - `rootfs-riscv64-lfs-<githubid>.tar.zst`

你需要在卷轴里提供:

- rootfs Release 页面链接
- rootfs 的 sha256

#### 禁忌

- **不要**在 rootfs 内放任何密钥、Token、SSH Key、`.git-credentials`、私钥文件、云服务配置等。
- **不要**把你机器上的真实 `/etc/ssh/`、`~/.ssh/`、`~/.config/` 之类目录不小心打包进去。

你在卷轴中需要做出声明 (模板里已写)。

#### 卷轴模板（旧版原文）

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

> 目标：从"下载 rootfs"到"进入环境的最短步骤。

### 方式 1

1. ...

<!-- 可选区 -->

## fastfetch / neofetch 证据

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

## 安全声明

- 我确认 rootfs 不包含任何密钥/Token/SSH Key/凭据/私人数据。
```

---

## 🎊 完成试炼后

恭喜你！当你完成这一切，你将：

1. 🎉 **获得 openRuyi 内部账号**，成为社区一员
2. 📖 **深入理解 Linux 系统构建**，这是宝贵的硬核技能
3. 🏗️ **有机会参与 openRuyi 发行版的开发**，创造历史
4. 🤝 **加入 RISC-V 生态建设**，与志同道合的伙伴同行

**无论选择了哪条路线，能够独立完成 LFS 都是了不起的成就！** 你证明了自己有能力从零构建一个操作系统，这是一项少数人拥有的能力。

### 💪 你掌握的核心能力

- **交叉编译技术** —— 嵌入式开发的基石
- **系统构建原理** —— 从源码到运行的完整流程
- **问题排查能力** —— 解决复杂技术问题的经验
- **文档阅读习惯** —— 从官方文档获取知识的能力

这些技能，无论是参与 openRuyi 开发，还是其他系统级项目，都将伴随你受益终身。

---

## 📎 拓展资源

想深入学习？这些资源会助你一臂之力：

- **[CLFS 项目](https://trac.clfs.org/)** - Cross Linux From Scratch 官方指南
- **[LFS 手册](https://linuxfromscratch.org/lfs/view/stable/)** - Linux From Scratch 权威文档
- **[RISC-V Machines](https://risc-v-machines.readthedocs.io/)** - RISC-V 虚拟化参考
- **[musl-cross-make](https://github.com/richfelker/musl-cross-make)** - 轻量级工具链方案

---

## 💡 核心理念

> **阶段隔离与验证密度**

通过严格区分 `cross-tools`（Host 工具）、`tools`（临时 Target 工具）与最终系统，确保每一步的错误不扩散；通过在工具链完成、Chroot 进入、系统启动三个关键节点设置硬性验证，避免在集成阶段遭遇不可调试的复合故障。

---

**相信自己，你已经准备好了！**

*期待在新手村见证你的成长，在 openRuyi 的城堡里与你相遇！* 🏰✨
