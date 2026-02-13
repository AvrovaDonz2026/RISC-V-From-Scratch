# ❓ 常见问题解答 (FAQ)

> 这里收集了历届试炼者遇到的典型问题及解决方案
> 
> 💡 **提示**：本文档基于 AvrovaDonz2026、Jvlegod、purofle 等同学的实际踩坑经验整理
> 
> 📌 **变量说明**：本文命令默认使用 `${CLFS}`。如果你的环境变量叫 `$LFS`，请按需替换。
> 
> 🎯 **路线建议**：openRuyi 最终技术栈是 `glibc + RPM`，遇到路线选择问题时，优先参考 glibc 方案。
>
> ⚠️ **代码示例声明**：本文所有命令与脚本片段均为实例，仅用于说明排障思路，不保证可直接在任意环境执行。
>
> ✍️ **文档署名**：本文档由 **Avrova Donz** 借助 **ChatGPT 5.3** 完成。

---

## 🔧 工具链构建问题

### Q1: GCC 编译报错 "flex: not found" / "bison: not found"

**症状**：
```
make: flex: Command not found
make: bc: Command not found
```

**原因**：缺少构建内核和 GCC 必需的工具。

**解决**：
```bash
# Ubuntu/Debian
sudo apt install flex bison bc

# Fedora
sudo dnf install flex bison bc
```

---

### Q2: GCC 编译报错 "cannot find cc1"

**症状**：
```
gcc: error trying to exec 'cc1': execvp: No such file or directory
```

**原因**：GCC 使用相对路径定位 cc1，交叉编译时路径解析失败。

**解决**：使用 `-B` 选项指定工具路径：
```bash
${TARGET}-gcc -B${CLFS}/cross-tools/libexec/gcc/${TARGET}/13.2.0/ ...
```

或在构建 GCC 时确保 `libexec` 目录结构正确。

---

### Q3: musl 动态链接器报 "not found"

**症状**：文件明明存在，但运行时报错：
```
/bin/ls: error while loading shared libraries: /lib/libc.so: cannot open shared object file: No such file or directory
```

**原因**：musl 的动态链接器使用绝对路径 `/lib/libc.so`，在 chroot 环境中解析失败。

**解决**：在 rootfs 中使用相对符号链接：
```bash
cd ${CLFS}/lib
ln -sf libc.so ld-musl-riscv64.so.1
```

**参考**：AvrovaDonz2026 的实践记录

---

### Q4: configure 报错 "cannot guess build type"

**症状**：
```
configure: error: cannot guess build type; you must specify one
```

**原因**：常见于 `config.guess/config.sub` 太旧，无法识别当前构建环境；或交叉编译参数里 `--build/--host` 填反。

**解决**：先更新 `config.guess/config.sub`，并确保 `--build` 是宿主机架构：
```bash
./configure \
  --build="$(gcc -dumpmachine)" \
  --host=riscv64-unknown-linux-gnu \
  ...
```

**参考**：purofle 在构建 Flex 时遇到

---

## 🚀 启动问题

### Q5: VFS: unable to mount root fs

**症状**：
```
VFS: Cannot open root device "vda" or unknown-block(0,0): error -6
Please append a correct "root=" boot option
```

**原因**：
1. 内核缺少 VirtIO 块设备驱动
2. `root=` 参数与设备名不匹配

**解决**：
1. 确保内核配置包含：
   ```
   CONFIG_VIRTIO_BLK=y
   CONFIG_VIRTIO_PCI=y
   ```

2. 检查 QEMU 参数，确保设备名匹配：
   ```bash
   # virtio 设备对应 /dev/vda
   -drive file=lfs-riscv64.img,format=raw,id=hd0,if=none \
   -device virtio-blk-device,drive=hd0
   -append "root=/dev/vda rw"
   
   # 或使用 SCSI（对应 /dev/sda）
   -drive file=lfs-riscv64.img,format=raw,if=scsi
   -append "root=/dev/sda rw"
   ```

---

### Q6: Chroot 后 "Exec format error"

**症状**：
```bash
chroot "${CLFS}" /tools/bin/bash
# => exec format error
```

**原因**：
1. `qemu-riscv64-static` 未复制到 chroot 环境
2. binfmt_misc 未注册或注册失败

**解决**：
```bash
# 1. 复制 QEMU 解释器
cp /usr/bin/qemu-riscv64-static ${CLFS}/usr/bin/

# 2. 注册 binfmt_misc（需要 root）
echo ':riscv64:M::\x7fELF\x02\x01\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x02\x00\xf3\x00:\xff\xff\xff\xff\xff\xff\xff\x00\xff\xff\xff\xff\xff\xff\xff\xff\xfe\xff\xff\xff:/usr/bin/qemu-riscv64-static:CF' > /proc/sys/fs/binfmt_misc/register

# 验证注册状态
cat /proc/sys/fs/binfmt_misc/riscv64
```

**补充**：如果已安装 `qemu-user-static` 包，可以：
```bash
# 检查服务状态
systemctl status systemd-binfmt.service

# 检查是否已注册
cat /proc/sys/fs/binfmt_misc/qemu-riscv64 | head -1
# 应显示: enabled
```

**参考**：purofle 的经验

---

### Q7: 内核启动后黑屏/无输出

**症状**：QEMU 启动后没有任何输出，或只有早期 printk。

**原因**：
1. 内核未启用串口控制台
2. 设备树与内核不匹配

**解决**：
```bash
# 1. 确保内核配置包含
CONFIG_SERIAL_8250=y
CONFIG_SERIAL_8250_CONSOLE=y
CONFIG_HVC_RISCV_SBI=y

# 2. 启动参数添加 earlycon
-append "earlycon=sbi console=ttyS0"

# 3. QEMU 参数确认
-nographic  # 或 -serial stdio
```

---

### Q8: 程序运行崩溃 / 无法启动 shell

**症状**：内核启动正常，但运行用户程序时崩溃或卡住。

**原因**：QEMU 版本与 glibc 版本不兼容。

**解决**：

**purofle 发现**：glibc 2.42 需要 QEMU 支持 termios2，否则程序会崩溃。

```bash
# 检查 QEMU 版本
qemu-system-riscv64 --version

# 如果使用的是 glibc 2.42+，需要 QEMU 10.1.2+ 或打补丁的版本
# 补丁地址:
# https://github.com/qemu/qemu/compare/v10.1.2...dramforever:qemu:add-termios2

# 临时解决：降级 glibc 到 2.40 或升级 QEMU
```

**参考**：purofle 的重要发现

---

## 📦 BusyBox 问题

### Q9: BusyBox tc applet 编译失败

**症状**：
```
tc.c: 错误: 'TCA_CBQ_MAX' 未声明
```

**原因**：BusyBox 1.36.1 的 `tc` (traffic control) applet 与 Linux 6.6.10+ 头文件不兼容。

**解决**：禁用 tc applet：
```bash
cd ${CLFS}/sources/busybox-1.36.1
make menuconfig
# Networking Utilities -> tc -> 取消选择

# 或直接修改 .config
sed -i 's/CONFIG_TC=y/CONFIG_TC=n/' .config
```

**参考**：AvrovaDonz2026 的解决方案

---

### Q10: BusyBox 静态链接失败

**症状**：
```
undefined reference to `__stack_chk_guard'
```

**原因**：某些宿主机启用了栈保护，但静态链接时缺少相关库。

**解决**：
```bash
# 在 BusyBox .config 中添加
CONFIG_EXTRA_CFLAGS="-fno-stack-protector"

# 或导出环境变量
export CFLAGS="-fno-stack-protector"
make -j$(nproc)
```

---

### Q11: /bin/sh: can't access tty; job control turned off

**症状**：启动后出现警告，但系统可以正常使用。

**原因**：在裸串口控制台（无 TTY 分配器）上运行时，shell 无法进行作业控制。

**解决**：这是预期行为，可以忽略。如需消除警告，确保：
```bash
# 1. /dev/console 设备存在
ls -la ${CLFS}/dev/console  # 应该有 c 5 1

# 2. init 正确启动 shell
exec /bin/sh -l  # 使用登录 shell
```

---

## 🔧 构建问题

### Q12: util-linux 安装报错 "chgrp: Operation not permitted"

**症状**：
```
chgrp: changing group of '/mnt/lfs/usr/bin/wall': Operation not permitted
make[4]: *** [Makefile:18850: install-exec-hook-wall] Error 1
```

**原因**：安装脚本尝试修改文件组，但在某些环境下权限不足。

**解决**：

**Jvlegod 发现**：此错误不影响核心工具的安装，可以忽略。

```bash
# 检查必要工具是否已安装
ls -l ${CLFS}/bin/mount
ls -l ${CLFS}/sbin/agetty
ls -l ${CLFS}/usr/lib/libmount.so.1

# 如果都存在，说明安装已成功，可以忽略 chgrp 错误
```

**参考**：Jvlegod 的经验

---

### Q13: 进入 chroot 后 "黑屏一片"

**症状**：chroot 成功后，没有任何提示符或输出。

**原因**：没有安装 shell（bash 或 busybox ash）。

**解决**：

**Jvlegod 提醒**：必须在 chroot 前确保目标系统里至少有一个可用 shell（`bash` 或 BusyBox `ash`）。

```bash
# 先检查 shell 是否存在
test -x ${CLFS}/bin/bash || test -x ${CLFS}/bin/sh || echo "缺少 shell"

# 若已安装 bash，确保 /bin/sh 指向可用 shell
ln -sf bash ${CLFS}/bin/sh

# 如果你走 BusyBox 路线，也可改为：
# ln -sf busybox ${CLFS}/bin/sh
```

---

### Q14: GCC Stage 2 构建后仍不支持 C++ 线程

**症状**：编译 C++ 程序时，`<thread>` 头文件找不到或链接失败。

**原因**：Stage 2 构建时未启用必要的库。

**解决**：

**Jvlegod 的 Stage 2 配置**：
```bash
../configure \
  --target=riscv64-unknown-linux-gnu \
  --prefix=${CLFS}/tools \
  --with-sysroot=${CLFS} \
  --disable-nls \
  --disable-multilib \
  --enable-languages=c,c++ \
  --enable-shared \
  --enable-threads=posix
```

其中最关键的是：
- `--enable-shared`
- `--enable-threads=posix`

---

## 🔄 Initramfs/Rootfs 切换问题

### Q15: pivot_root 失败

**症状**：
```bash
pivot_root: Invalid argument
```

**原因**：
1. 新 root 不是挂载点
2. 旧 root 有繁忙的挂载点

**解决**：
```bash
# 正确流程：确保新 root 是一个挂载点
mount --bind ${NEW_ROOT} ${NEW_ROOT}
pivot_root ${NEW_ROOT} ${NEW_ROOT}/mnt
exec chroot . /bin/sh < /dev/console > /dev/console 2>&1
```

或使用 `switch_root`（BusyBox 提供）：
```bash
# 在 initramfs 中
mount -t devtmpfs devtmpfs /dev
# ... 找到真实 root 并挂载到 /newroot ...
switch_root /newroot /sbin/init
```

---

### Q16: initramfs 到 rootfs 切换后服务无法启动

**症状**：切换后系统无法找到设备、网络不工作等。

**原因**：initramfs 中的设备节点、挂载信息未正确传递。

**解决**：
```bash
# 1. 确保 devtmpfs 已挂载
mount -t devtmpfs devtmpfs /dev

# 2. 转移关键挂载点
mount --move /dev /newroot/dev
mount --move /proc /newroot/proc
mount --move /sys /newroot/sys
```

---

## 🌐 网络配置问题

### Q17: QEMU 中网络不通

**症状**：无法 ping 通外网。

**解决**：

**purofle 的完整方案**：

```bash
# 1. 启动 QEMU 时添加网络设备
qemu-system-riscv64 \
    ... \
    -netdev user,id=net0,hostfwd=tcp::2222-:22 \
    -device virtio-net-device,netdev=net0

# 2. 确保内核支持 VirtIO_NET
CONFIG_VIRTIO_NET=y

# 3. 在 guest 中配置网络
ip link set dev eth0 up
dhcpcd eth0  # 或使用: ip addr add 10.0.2.15/24 dev eth0

# 4. 配置 DNS
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

---

### Q18: curl 访问 https 网站报错

**症状**：
```
curl: (60) SSL certificate problem: unable to get local issuer certificate
```

**原因**：缺少 CA 根证书。

**解决**：

**purofle 建议**：先编译安装 `make-ca` 及其依赖。

```bash
# 参考 BLFS: https://linuxfromscratch.org/blfs/view/systemd/postlfs/make-ca.html
# 安装后更新证书
make-ca -g
```

---

## 📋 提交相关问题

### Q19: rootfs 文件应该多大？

**建议大小**：
- **最小系统**（BusyBox static）：~5-10MB
- **带 glibc 的基础系统**：~50-100MB
- **包含基本工具链**：~200-500MB

**实际数据**：
| 同学 | C库 | Init | 大小 |
|------|-----|------|------|
| AvrovaDonz2026 | musl | BusyBox | ~2MB |
| Jvlegod | glibc | SysVinit | ~100MB+ |
| purofle | glibc | systemd | ~500MB+ |

**优化**：
```bash
#  strip 符号
find ${CLFS}/bin ${CLFS}/sbin ${CLFS}/usr/bin ${CLFS}/usr/sbin -type f \
  -exec ${TARGET}-strip --strip-unneeded {} + 2>/dev/null || true

# 删除静态库（如果不需要编译）
rm -rf ${CLFS}/usr/lib/*.a

# 使用 zstd 高压缩
tar -cf rootfs.tar -C ${CLFS} .
zstd -T0 --ultra -22 rootfs.tar
```

---

### Q20: 如何生成 SHA256？

```bash
GITHUB_ID=your_github_id

# Linux
sha256sum rootfs-riscv64-lfs-${GITHUB_ID}.tar.zst

# macOS
shasum -a 256 rootfs-riscv64-lfs-${GITHUB_ID}.tar.zst
```

---

## 💡 调试技巧

### 查看 ELF 依赖

```bash
# 查看动态链接器路径
readelf -l /path/to/binary | grep interpreter

# 查看动态库依赖
readelf -d /path/to/binary | grep NEEDED

# 使用交叉 readelf
${TARGET}-readelf -a /path/to/binary
```

### 启用详细日志

```bash
# 内核启动日志
-append "debug initcall_debug loglevel=8"

# strace（需要编译安装）
strace -f /bin/command

# BusyBox 调试
sh -x /init
```

### QEMU 调试

```bash
# 停止在第一条指令
-S -s

# 然后使用 GDB 连接
target remote localhost:1234

# 单核调试（避免多核干扰）
-smp 1
```

---

## 🆘 如果以上都无法解决

别担心，遇到困难是正常的！每个完成 LFS 的人都曾经历过这些。试试这些方法：

1. 🔍 **仔细检查命令** —— 复制粘贴有时会有隐藏字符
2. 📋 **查看日志** —— `dmesg` 和 `/var/log/` 会告诉你真相
3. 👀 **对比成功案例** —— `submissions/` 目录下有很多参考
4. 🎯 **逐步回退** —— 一次只改一个地方，缩小问题范围
5. 🤝 **寻求帮助** —— 在 PR 中描述问题，社区会支持你

**记住**：每一个问题都是学习的机会。当你解决它，你就比昨天更强了！

---

*这份 FAQ 凝聚了众多先行者的经验 —— 你并不孤单！* 💪
