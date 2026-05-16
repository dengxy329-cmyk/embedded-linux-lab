# 00-env / 环境地基（M1）

> 改一行驱动代码到板子 `dmesg` 看到结果 **< 2 分钟** —— 这是 M1 唯一交付物。
> 后面所有 M 阶段都吃这个环境，磨刀不误砍柴工。

## 这个目录干啥

把"VM + 跨编译 + 板子 + 串口 + NFS root + 阅读工具 + git 工作流"七件套一次性配齐，建立 ROADMAP §7 的高速迭代环。

## 文件清单

| # | 文件 | 内容 | 状态 |
|---|------|------|------|
| 1 | `vm-and-host.md` | VMware Workstation 网络（**必须桥接**）/USB 透传/共享文件夹 | ⏳ |
| 2 | `cross-toolchain.md` | Bootlin aarch64 toolchain 安装 + env 脚本 | ⏳ |
| 3 | `serial-console.md` | USB 转串口透传 VM、ttyUSB0 udev 规则、minicom/picocom | ⏳ |
| 4 | `rk3568-bringup-itop.md` | 讯为板子针脚、SD 卡烧录、U-Boot 进入 | ⏳ |
| 5 | `nfs-root-loop.md` ⭐ | **M1 真瓶颈**：NFS server + TFTP + U-Boot bootargs 一条龙 | ⏳ |
| 6 | `code-reading-tools.md` | Elixir / clangd + bear / ctags / cscope | ⏳ |
| 7 | `git-kernel-workflow.md` | format-patch / send-email / checkpatch.pl | ⏳ |
| 8 | `qcs6490-bringup-radxa.md` | Radxa Dragon Q6A（fastboot/edl、Qualcomm 流程） | ⏳ |

## 推荐写作顺序（也是动手顺序）

| 顺序 | 文件 | 工时 | 卡点 |
|------|------|------|------|
| 1 | `vm-and-host.md` | 半天 | VM 网卡桥接、Win 防火墙 |
| 2 | `cross-toolchain.md` | 1 小时 | Bootlin 下载慢，挂代理 |
| 3 | `serial-console.md` | 半天 | USB 透传给 VM、ttyUSB0 权限 |
| 4 | `rk3568-bringup-itop.md` | 1 天 | BSP defconfig 选哪个 |
| 5 | `nfs-root-loop.md` ⭐ | **2 天** | bootargs 一行 80+ 字符容易抄错 |
| 6 | `code-reading-tools.md` | 半天 | clangd 索引内核要 `bear -- make` |
| 7 | `git-kernel-workflow.md` | 半天 | send-email 配 Gmail App Password |
| 8 | `qcs6490-bringup-radxa.md` | 拿到板再写 | 等硬件 |

## M1 自检（对应 ROADMAP §11）

- [ ] RK3568 串口能稳定通信
- [ ] VM 编出的 .ko 板子能立刻 `insmod`
- [ ] **改一行代码 → 板子看到效果 < 2 分钟**
- [ ] 能从 dts 找到对应驱动文件
- [ ] git format-patch 流程能跑通

## 工程视角的真坑（教科书不会教）

| # | 坑 | 表现 | 解 |
|---|----|------|---|
| 1 | VMware 默认 NAT 模式 | 板子 ping 不到 VM（NAT 只让 VM 单向出去） | 网卡改**桥接（Bridged）**，让 VM 和板子在同一物理网段 |
| 2 | VMware USB 控制器没开 | USB 转串口插上 VM 看不到 `/dev/ttyUSB0` | VM 设置 → USB Controller → 兼容性 USB 2.0/3.x；插上后 VM 右下角点 USB 图标连过来 |
| 3 | VM 防火墙拦 NFS 端口（111, 2049） | 板子 mount 卡死 | VM 里 `sudo ufw disable` 或显式开 NFS 端口 |
| 4 | NFS 导出忘了 `no_root_squash` | 板子能挂上但写文件 Permission denied | `/etc/exports` 加 `no_root_squash,no_subtree_check` |
| 5 | NFS 用 `async` 还是 `sync` | sync 慢、async 偶发数据丢 | 学习场景用 `async`，验证稳定后再考虑 `sync` |
| 6 | U-Boot 默认 `bootcmd` 太快 | 串口来不及按键中断 | `setenv bootdelay 5 && saveenv` |
| 7 | U-Boot env 不持久 | 重启又回默认 | `saveenv` 写回 SPI Flash/eMMC |
| 8 | BSP defconfig 选错 | 编出来的 Image 板子起不来 | 用 `rockchip_linux_defconfig`，不是主线 `defconfig` |
| 9 | 跨编译 sysroot 缺 | 链接报 `cannot find -lc` | 用 Bootlin **完整** toolchain，不用 apt 的 `gcc-aarch64-linux-gnu` |
| 10 | Bootlin toolchain 版本太新 | 内核某些版本编不过（GCC 13 + 老内核） | 内核 v6.6 用 GCC 12 或 13，不要追最新 |
| 11 | tftpd-hpa 权限 | `boot tftp` 报 timeout 或 "File not found" | `/srv/tftp` 权限 755 + 文件 644 + selinux/apparmor 关 |
| 12 | 板子静态 IP 与 VM 不同网段 | 互相 ping 不到 | 桥接模式下 VM 和板子必须同网段，IP 段看路由器 DHCP 池 |
| 13 | 板子 IP DHCP 每次变 | NFS bootargs 写死会失效 | 板子设静态 IP（U-Boot `setenv ipaddr`） |
| 14 | clangd 不识别内核宏 | VSCode 跳转失败、红波浪 | 先 `bear -- make ARCH=arm64 CROSS_COMPILE=...` 生成 compile_commands.json |
| 15 | 写 .ko 装到错路径 | 板子 `modprobe` 找不到 | `make modules_install INSTALL_MOD_PATH=<NFS-rootfs 路径>` |
| 16 | Ubuntu 18.04 已 EOL | apt 装新工具碰到 repo 失效 | 推荐至少 20.04，最好 22.04/24.04；老 18.04 VM 留给特殊兼容场景 |
