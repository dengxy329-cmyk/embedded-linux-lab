# 70-build-systems / 构建系统（M6）

> Kbuild → Buildroot → Yocto → U-Boot → mainline 对比，依次摸过去。
> 目标：把"vendor BSP 给你打好的包"全部撕开看，自己造一遍。

## 这个目录干啥

M1 你用的是 vendor BSP 的 `build.sh` 一键脚本。
M6 要把每一层都拆开：

| 层 | M1 的做法 | M6 的做法 |
|----|----------|----------|
| 跨编译 | Bootlin tarball | 还是 Bootlin（这个不变） |
| 内核 build | `make rockchip_linux_defconfig && make` | Kbuild 完整理解 + 自定义 Kconfig |
| rootfs | NFS 临时挂 | Buildroot / Yocto 生成完整 image |
| U-Boot | vendor 预编译 | 自己 patch、自己编 |
| 烧录 | upgrade_tool 一键 | dd / fastboot / rkdeveloptool 手动 |

## 文件清单

| 文件 | 内容 | 板子 | 状态 |
|------|------|------|------|
| `kbuild-kconfig.md` | Makefile.build、`obj-y` / `obj-m`、Kconfig 语法、`select`/`depends on` | 通用 | ⏳ |
| `buildroot-rk3568.md` | Buildroot 给 RK3568 生成 rootfs，加自己的 .ko | RK3568 | ⏳ |
| `buildroot-qcs6490.md` | Buildroot 给 Q6A 生成 rootfs，对比 Rockchip 流程 | QCS6490 | ⏳ |
| `yocto-quickstart.md` | meta-rockchip / meta-qcom 体验，bitbake 基础 | 两板 | ⏳ |
| `uboot-on-rk3568.md` | U-Boot 编译、改 env、改 bootcmd、烧录 | RK3568 | ⏳ |
| `bsp-vs-mainline.md` | BSP 跟主线差在哪、怎么对齐、什么时候必须用 vendor | 通用 | ⏳ |

## 学习节奏

| 周 | 任务 |
|----|------|
| W1 | Kbuild + Kconfig 写一个自定义子目录的 Makefile + 加进顶层 |
| W2 | Buildroot RK3568 完整 image，加 BusyBox + ssh + 自己的模块 |
| W3 | 主线 Linux 编译 + 跑在 RK3568（不用 BSP，看主线缺什么） |
| W4 | Yocto + meta-rockchip 体验一次（理解 layer / recipe / image） |
| W5 | Qualcomm 路线：meta-qcom / fastboot / edl 烧录 |
| W6 | bsp-vs-mainline 总结对比表 |

## 必须装的工具

```text
bear                      生成 compile_commands.json
ccache                    加速重编
buildroot >= 2024.02      LTS 版本
qemu-system-aarch64       本地验证 image
fastboot / rkdeveloptool  烧录
device-tree-compiler      dtc
```

## 铁律提醒

- §8.4 永远跑在真硬件上：QEMU 跑通的 image 不等于板子能起
- §8.6 先 work 再 right 再 fast：Buildroot 先 default 跑通，再裁剪
- §8.7 不抄代码要内化：抄完 `meta-rockchip` 必须自己能讲清 layer 怎么挂

## M6 自检（ROADMAP §11）

- [ ] Buildroot 给自己板子做出过完整 rootfs
- [ ] 提过至少 1 个主线 patch（哪怕被 reject 也算）
- [ ] checkpatch.pl 跑过自己所有代码
- [ ] 主线 Linux 至少跑过一次 RK3568（哪怕功能不全）
