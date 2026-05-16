# Linux 驱动工程师学习宪章

> 这是一份"路线 + 方法 + 上下文"的总纲，给学习者本人 + 任何接续协助的 AI 模型阅读。  
> 创建：2026-05-16  
> 维护者：dengxy329-cmyk  
> 仓库：<https://github.com/dengxy329-cmyk/linux-kernel-learning>

---

## 0. 这份文档怎么用

### 给学习者本人

- 这是你的**学习宪章**——大方向迷茫时回来看
- 进度看板：每完成一个 M 阶段，在 §11 表格里勾选
- 老笔记在 `legacy-textbook-structure` 分支；新结构在 `main`

### 给协助的 AI 模型（接续上下文用）

请先通读本文档，重点理解：

1. §1 学习者画像——他不是初学者，是有教科书基础但跨不到工程实践的人
2. §3-§4 路线——按"能力"分阶段，不按"知识章节"分阶段
3. §5 方法论——核心 4 套方法，不要回到"按 OSTEP/APUE 章节讲"的老路
4. §8 核心原则——你给的建议必须符合这些原则
5. §9 Rockchip vs Qualcomm 差异——他两套板子都有，建议要区分

**关键禁忌（曾经踩过的坑）**：

- ❌ 不要再按 OSTEP/APUE 章节做笔记——这些他已经有完整资料
- ❌ 不要从"写 cdev hello world"开始——他已会
- ❌ 不要堆砌"理论 + 教科书例子"——他要工程实践
- ✅ 直接从"读真实驱动代码"切入
- ✅ 每个建议都要有"在 RK3568 / QCS6490 上怎么做"
- ✅ 输出尽量是 markdown 笔记格式，方便他直接存档

---

## 1. 学习者画像

| 项 | 内容 |
|----|------|
| **身份** | 研一在读，端侧 AI 公司实习生 |
| **C 语言** | 中等：指针/结构体/函数指针/位操作熟练 |
| **OS 理论** | 已补齐：OSTEP 全本读过、有完整笔记 |
| **用户态编程** | 中等：APUE 应用层、fork/exec/wait/pthread/socket 都会 |
| **教科书驱动** | 学过：会写最小 cdev、知道 module_init/exit、file_operations |
| **硬件底子** | STM32/Arduino 单片机经验，懂寄存器/GPIO/I2C/SPI/UART |
| **核心痛点** | **看公司真实代码懵**——平台驱动、子系统、devm_*、设备树串起来理不清 |
| **职业目标** | 短期：能胜任初级 Linux 驱动工程师<br>中期：AI 端侧（Camera/ISP/NPU）专精<br>长期：车载 AI 视觉方向 |
| **每周时间** | 20 小时以上（实习 + 自学） |
| **学习偏好** | 笔记要直白精简、有面试题答案、不要堆砌理论 |

### 优势

- 硬件底子 + 单片机经验，不怕寄存器、不怕示波器
- OS 理论扎实，能立刻理解为什么需要 mmap / spinlock / vmalloc
- 公司有真实项目可观察
- 板子资源丰富

### 短板

- 没读过大型代码库的训练
- 没用过工业级调试工具（ftrace / bpftrace / kgdb）
- 没提过 patch、不熟邮件列表文化
- 没碰过完整 BSP 构建（Yocto / Buildroot）

---

## 2. 现有资源

### 硬件

| 平台 | 用途 | 备注 |
|------|------|------|
| **讯为 iTOP-RK3568 开发板** | 主学习板（M1-M6） | 资料齐全、社区活跃、mainline 支持好 |
| **Radxa Dragon Q6A (QCS6490)** | AI 端侧实战 | 12 TOPS NPU；Qualcomm 路线起步 |
| **Qualcomm QCS8300 项目板**（未来） | 车载 AI 相机 | 公司项目，专精阶段（M7）介入 |

### 软件环境

- 主机：Windows + WSL2（应用层用）
- 开发：Ubuntu 22.04 VM（VirtualBox，驱动主战场，4 核 / 8 GB / 100 GB）
- IDE：VSCode / Cursor + clangd
- 已装：git、gcc、build-essential
- 待装（M1）：Bootlin aarch64 toolchain、NFS 服务、minicom/picocom、ctags、cscope

### 学习资料

- OSTEP（已读完）
- APUE / TLPI（参考）
- 尚硅谷 Linux 应用层开发文档（已有，应用层不再重复造轮子）
- 主线 Linux v6.6 源码（待 clone）
- Rockchip BSP（待 clone：<https://github.com/rockchip-linux/kernel>）
- Qualcomm Linaro：<https://git.codelinaro.org/clo/la/kernel>
- LWN 周刊（订阅，每周读）

---

## 3. 路线总框架

```text
M0  教科书阶段（已完成）
    OS 理论 + 用户态系统编程 + 教科书 cdev 驱动

────── 分水岭：从"会基础"到"工程能用" ──────

M1  环境地基（2 周，全力以赴）
M2  阅读训练营（3-4 周）
M3  修改训练（3-4 周）
M4  调试武器化（贯穿 M2-M3 + 集中 1 周）
M5  子系统按 AI 端侧路线点亮（持续，6-12 周）
M6  工程实践与对外贡献（穿插）
M7  方向专精：AI Camera/ISP/NPU（6 个月之后）
```

**总时长估算**：达到"初级 Linux 驱动工程师能上岗"约 **5-7 个月**（每周 20 小时投入）。

**核心理念**：

| 教科书顺序（错） | 工程顺序（对） |
|----------------|--------------|
| 学一个 API → 写一个示例 | 读真实代码 → 模仿 → 改 → 写 |
| 从字符设备开始 | 从平台驱动 + 设备树开始 |
| printk 调试为主 | ftrace/bpftrace 工业级调试 |
| Makefile hello world | Kbuild/Kconfig + 跨编译 + NFS root |

---

## 4. 各阶段详细

### M1：环境地基（2 周）

**目标**：建立"2 分钟迭代环"——改代码 → 编 → 部署到板 → 看 dmesg。

**Deliverables**：

1. Ubuntu VM 装好、跨编译工具链就位
2. RK3568 板子能稳定上电、串口可控、网络通
3. **NFS root**：板子根文件系统挂在 VM 上，VM 上编完模块板子立刻看到
4. Elixir / clangd 阅读环境
5. git 内核 patch 工作流（format-patch / send-email / Signed-off-by）能跑通

**关键技能**：

- Bootlin 预编译 toolchain 安装与验证
- 内核 defconfig 选 RK3568 + 编译 Image + dtb
- U-Boot 从 SD / TFTP 加载内核
- minicom / picocom 串口操作
- NFS server (VM) + NFS client (board) 配置
- ssh 互通（密钥认证）

**自检**：从 VM 上 `vim` 改一行驱动代码到板子 `dmesg` 看到结果，**< 2 分钟**。

---

### M2：阅读训练营（3-4 周） ⭐ 分水岭

**目标**：会读真实驱动代码，建立"模式库"。

**方法**：5-8 个 case study，每个用 §5.1 七步法，mainline + BSP 对比看。

**推荐 case 列表**（由浅入深）：

| # | Case | 大小 | 学什么 |
|---|------|------|--------|
| 01 | `drivers/gpio/gpio-rockchip.c` | ~600 行 | platform driver 骨架、of_match、devm_* |
| 02 | `drivers/i2c/busses/i2c-rk3x.c` | ~1500 行 | I2C controller 驱动、中断、时序 |
| 03 | `drivers/iio/imu/bmi160/` | ~1500 行 | 子系统驱动（IIO）、sysfs |
| 04 | `drivers/spi/spi-rockchip.c` | ~1200 行 | SPI controller、DMA |
| 05 | `drivers/pwm/pwm-rockchip.c` | ~500 行 | 简单子系统的完整例子 |
| 06 | `drivers/media/platform/rockchip/rkisp1/` | 大 | Camera/ISP（贴 AI 方向） |
| 07 | 公司项目代码（脱敏后） | 视情况 | 实战拆解 |
| 08 | Qualcomm QCS6490 上某个简单驱动 | 视情况 | 对比 Rockchip 风格 |

**输出**：每个 case 一个目录（见 §6），含 dts 追踪、probe 解析、调用图、自己的疑问。

**禁忌**：M2 期间**不写新代码**，只读+笔记。这是反直觉但有效的训练。

---

### M3：修改训练（3-4 周）

**目标**：会在已有驱动上"外科手术"。

**练习类型**（在 M2 读过的驱动上做）：

- 加 sysfs 节点暴露内部状态
- 加 module_param 让用户调参
- 加更细的 dynamic debug 日志
- 适配自己板上的新硬件（如挂个传感器）
- 改时序、改频率、改寄存器顺序观察影响
- 把一个 BSP 驱动**向主线靠拢**（删冗余、补 devm_*）

**Deliverables**：每个修改一个 commit + 一篇"我改了什么、为什么"的笔记。

---

### M4：调试武器化（贯穿 + 集中 1 周）

**目标**：每个调试工具至少破解过一个真 bug。

| 工具 | 场景 | 典型练习 |
|------|------|---------|
| `printk` / dynamic debug | 日志追踪 | 用 `dyndbg=+pf` 启动看具体函数日志 |
| `ftrace` events | 子系统事件 | 抓 i2c_read/i2c_write 事件 |
| `ftrace` function/function_graph | 函数级时序 | 看 probe 走了哪些函数、各自耗时 |
| `bpftrace` | 一行命令 | `tracepoint:i2c:i2c_read { printf(...) }` |
| `perf top/trace` | 性能采样 | 看哪个内核函数 CPU 占比高 |
| `kgdb` | 板上调内核 | 串口 + kgdb，断点 probe |
| oops / panic 分析 | 崩溃排查 | 手工算偏移找到崩溃点 |
| `crash` 工具 | dump 分析 | 看现场寄存器/栈/链表 |

**输出**：`60-debug-arsenal/war-stories/` 每个工具至少一篇"我用 XX 排掉了 YY"。

---

### M5：子系统按 AI 端侧路线点亮（持续）

**目标**：把端侧 AI 链路上每个子系统的"框架感"建立起来。

**推荐顺序（按依赖链）**：

```text
GPIO / Pinctrl                ← 最基础，理解子系统模板
    ↓
Clock / Regulator / Power     ← 时钟/电源/功耗管理
    ↓
I2C / SPI / UART              ← 经典慢速总线
    ↓
IIO                           ← 传感器框架（陀螺仪、气压计等）
    ↓
V4L2 / Camera / ISP           ← ⭐ AI 视觉核心
    ↓
DMA-BUF / dma-heap            ← ⭐ AI 内存共享核心
    ↓
DRM / KMS                     ← 显示输出（必要时）
    ↓
RKNPU / QCS NPU 闭源驱动逆向   ← AI 加速器接入
```

每个子系统三件事：

1. 画框架图（核心结构体、注册流程、调用关系）
2. 读一个典型实现（mainline + BSP 对比）
3. 自己改一个或写一个简化版

**说明**：

- 不必每个都做"完"，按需点亮
- AI 端侧最重要的是 **V4L2 + DMA-BUF**，到这一步要重锤砸
- RKNPU 闭源，方法是看用户态 SDK 的 ioctl 调用反推 driver 接口

---

### M6：工程实践与对外贡献（穿插）

**目标**：摸到"工业级 Linux 驱动开发流程"。

**清单**：

- [ ] Buildroot 给 RK3568 生成一个完整 rootfs（自定义 BusyBox、加自己的模块）
- [ ] Yocto 体验一次（用 meta-rockchip 或 meta-qcom）
- [ ] U-Boot 编译、修改、烧录
- [ ] 主线 Linux 编译跑在 RK3568（不用 BSP，看主线纯净版能干什么）
- [ ] `checkpatch.pl` 跑过自己的代码
- [ ] `git format-patch` + `git send-email` 流程跑通
- [ ] **提一个主线 patch**（哪怕只是修个 typo / 改个 dt-bindings 文档）
- [ ] 订阅并能跟读 LKML 某个子系统（如 linux-rockchip / linux-arm-msm）

---

### M7：方向专精（6 个月后）

**首选方向（基于内推+板子）**：AI Camera / ISP / NPU。

**子专题**：

```text
Camera 路线
├── V4L2 框架深入
├── MIPI CSI 协议
├── Rockchip rkisp1 / Qualcomm Spectra ISP
├── 多摄像头同步、HDR、AE/AWB
└── ISP tuning（涉及色彩、降噪、锐化算法）

NPU 路线
├── DMA-BUF / dma-heap 内存框架
├── IOMMU（AI 加速器都需要）
├── RKNPU SDK 反向研究
├── Qualcomm Hexagon DSP/NPU
└── 用户态推理框架（RKNN / SNPE / QNN）和 driver 接口

DMA 与内存
├── DMA Mapping API
├── IOMMU framework
├── DMA-BUF 共享 / sync / fence
└── 大页 / Contiguous Memory Allocator (CMA)
```

**长线（车载 AI）**：

- AUTOSAR Classic / Adaptive 基础认知
- Functional Safety (ISO 26262) 概念
- Real-time Linux (PREEMPT_RT)
- ROS2（很多 ADAS 用）

---

## 5. 学习方法论

### 5.1 读驱动 7 步法

```text
1. 找入口     dts → 看 compatible
2. 找驱动     compatible → grep "of_match" / "of_device_id"
3. 看 probe   逐行：声明了什么资源？注册了什么？
4. 看 ops     fops / 子系统 ops，用户态怎么用
5. 追一次     挑一个 read/write/ioctl，从用户态追到硬件寄存器
6. 画图       mermaid 或纸笔，画调用关系
7. 写笔记     惊讶点 / 模式 / 工程坑
```

**禁止**：从第一行读到最后一行。源码是图，不是书。

### 5.2 改驱动 5 步法

```text
1. 找参考     找一个最相似的、能跑的代码
2. 复制改名   整文件 cp 一份，namespace 全部改干净
3. 最小改动   先只改一个地方，立刻部署测
4. 跑通基线   保证没坏（这步省略 = 后面 debug 翻倍）
5. 增量迭代   每次只改一件事
```

### 5.3 调试 5 步法

```text
1. 能稳定复现     不能复现的 bug 等于不存在
2. 二分           时间二分 / 代码二分 / 输入二分
3. 选对工具       时序问题 ftrace / 状态问题 kgdb / 性能 perf
4. 假设-验证      一次一个假设，立刻验
5. 写下来         修完写"为什么"，不是"修了什么"
```

### 5.4 笔记 "案例驱动" 法

不按章节记笔记，按案例记。每个 case 一个目录：

```text
case-NN-xxx/
├── 00-overview.md       这个驱动干啥、为什么挑它
├── 01-dts.md            dts 节点 + 对应 bindings
├── 02-walkthrough.md    probe 逐段解析
├── 03-trace.md          一次操作的完整调用栈
├── 04-modifications.md  我对它做了什么、为什么
├── 05-debug-log.md      碰到的坑、怎么排的
└── refs/                参考过的 commits / LWN / 邮件
```

---

## 6. 仓库结构

```text
linux-kernel-learning/                  ← main 分支
├── ROADMAP.md                          ← 本文档（学习宪章）
├── README.md                           ← 进度看板 + 路线一图
│
├── 00-env/                             ← M1：环境
│   ├── README.md
│   ├── vm-and-host.md
│   ├── rk3568-bringup-itop.md          ← 讯为 RK3568 上电
│   ├── qcs6490-bringup-radxa.md        ← Radxa Dragon Q6A 上电
│   ├── cross-toolchain.md
│   ├── nfs-root-loop.md                ← ⭐ 高速迭代环
│   ├── serial-console.md
│   ├── code-reading-tools.md           ← Elixir/ctags/cscope/clangd
│   └── git-kernel-workflow.md
│
├── 10-fundamentals/                    ← 必备速查（不是讲义）
│   ├── memory-and-io.md                ← ioremap、PHYS/VIRT、cache
│   ├── concurrency.md                  ← spinlock/mutex/RCU 一表
│   ├── interrupt-model.md
│   ├── kernel-objects.md               ← kobject/device/driver
│   └── modern-patterns.md              ← devm_*/IS_ERR/ERR_PTR/cleanup
│
├── 20-driver-model/                    ← M2 前置（薄）
│   ├── platform-driver.md
│   ├── of-and-bindings.md
│   ├── probe-failure-and-rollback.md
│   └── sysfs-debugfs.md
│
├── 30-reading-drills/                  ← ⭐⭐⭐ M2 主战场
│   ├── case-01-gpio-rockchip/
│   ├── case-02-i2c-rk3x/
│   ├── case-03-iio-bmi160/
│   ├── case-04-spi-rockchip/
│   ├── case-05-pwm-rockchip/
│   ├── case-06-rkisp1-camera/
│   ├── case-07-internal-xxx/           ← 公司代码脱敏
│   └── case-08-qcs6490-yyy/
│
├── 40-modification-drills/             ← M3
│   ├── add-sysfs-to-gpio-rockchip/
│   ├── port-bmi160-to-itop-rk3568/
│   └── ...
│
├── 50-subsystems/                      ← M5：按需点亮
│   ├── gpio/
│   ├── pinctrl/
│   ├── clk-and-power/
│   ├── i2c-and-iio/
│   ├── spi/
│   ├── v4l2-camera/                    ← AI 视觉核心
│   ├── drm-display/
│   ├── dma-buf/                        ← AI 内存核心
│   ├── iommu/
│   ├── rknpu-research/                 ← 闭源逆向笔记
│   └── qcom-hexagon-research/
│
├── 60-debug-arsenal/                   ← M4
│   ├── printk-and-dynamic-debug.md
│   ├── ftrace.md
│   ├── perf.md
│   ├── bpftrace-oneliners.md
│   ├── kgdb-on-rk3568.md
│   ├── oops-panic-cookbook.md
│   ├── crash-tool.md
│   └── war-stories/
│       ├── 001-xxx.md
│       └── ...
│
├── 70-build-systems/                   ← M6
│   ├── kbuild-kconfig.md
│   ├── buildroot-rk3568.md
│   ├── buildroot-qcs6490.md
│   ├── yocto-quickstart.md
│   ├── uboot-on-rk3568.md
│   └── bsp-vs-mainline.md
│
├── 80-patches/                         ← M6：对外贡献
│   ├── 001-typo-xxx/
│   │   ├── 0001-xxx.patch
│   │   ├── cover-letter.txt
│   │   └── lkml-thread.md
│   └── ...
│
├── 90-interview-prep/                  ← 求职/晋升
│   ├── q-platform-driver.md
│   ├── q-concurrency.md
│   ├── q-memory-and-dma.md
│   ├── q-debug-scenarios.md
│   ├── q-ai-camera-specific.md
│   └── system-design.md
│
└── 99-resources/
    ├── books-and-articles.md
    ├── mailing-lists.md
    ├── youtube-conference-talks.md
    └── cheatsheets/
        ├── git-kernel.md
        ├── devm-functions.md
        ├── of-property-read.md
        └── ftrace.md
```

---

## 7. 日常工作流

### 高速迭代环（M1 必须建好）

```text
[Windows 主机]
  └ VSCode / Cursor 编辑（远程到 VM）
  └ 浏览器 Elixir 读 mainline

[Ubuntu VM]
  ├ 内核 / 模块源码
  ├ aarch64 跨编译工具链
  ├ NFS server → 板子的 rootfs 共享
  ├ TFTP server → 内核/dtb 拉取
  └ ssh / scp / minicom

[RK3568 / QCS6490 板]
  ├ U-Boot 启动 → TFTP 拉 kernel + dtb
  ├ NFS root 挂载 VM 上的 rootfs
  ├ 串口控制台
  ├ ssh 互通
  └ insmod xxx.ko → dmesg
```

### 一次迭代的全流程

```bash
# 在 VM 里
vim drivers/.../my_driver.c
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- M=drivers/.../ modules
# 输出的 .ko 自动落在板子 NFS 挂载的目录里

# 在板子（ssh 或串口）
rmmod my_driver
insmod /lib/modules/.../my_driver.ko
dmesg | tail -20
```

**目标耗时：< 2 分钟**。环境对路了，这是日常节奏。

---

## 8. 核心原则（10 条铁律）

1. **永远先读后写**——新功能先去主线/vendor 找最像的代码
2. **永远用 devm_***——拒绝裸 `kmalloc + kfree`、`ioremap + iounmap`、`request_irq + free_irq`
3. **永远写好 commit message**——subject + body + Signed-off-by，日常习惯就按内核 patch 风格
4. **永远跑在真硬件上**——QEMU 通过 ≠ 真板能跑；驱动是和硬件谈判
5. **永远写"为什么"**——代码已经说了"是什么"，注释/笔记/commit body 只说"为什么"
6. **永远先简单后正确后快**——make it work → make it right → make it fast，**不要反过来**
7. **不抄代码，要内化**——抄完一段必须能合书复述思路
8. **永不破坏用户态 ABI**——Linus 的铁律
9. **错误路径必须干净**——probe 失败要释放所有已申请的资源（devm_* 帮你大忙）
10. **不要怕板子崩**——内核 panic 是日常，snapshot/重烧固件就行

---

## 9. Rockchip vs Qualcomm 工程差异（重要）

你两套板子都有，工程实践完全不同，必须区分：

| 维度 | Rockchip (RK3568) | Qualcomm (QCS6490 / 8300) |
|------|-------------------|---------------------------|
| **BSP 开放度** | GitHub 全开 (rockchip-linux/kernel) | CodeLinaro，分仓多，部分需注册 |
| **主线支持** | 好（RK3568/3588 都已 mainline） | 差，QCS 系列主要靠 downstream |
| **设备树** | 干净规范 | 厂商 patch 多，复杂 |
| **构建系统** | Buildroot / Yocto / Android 都行 | **Yocto + meta-qcom 主导** |
| **安全启动** | 可选 | **强制启用**，Secure Boot + ATF 严格 |
| **TrustZone** | 一般少用 | **重度使用**，TEE/OP-TEE 是标配 |
| **Camera/ISP 驱动** | rkisp1 开源 | Spectra ISP **闭源** |
| **NPU/DSP 驱动** | RKNPU 部分闭源 | Hexagon **完全闭源**，SDK 接入 |
| **GPU** | Mali (Panfrost 开源) | Adreno (freedreno mainline) |
| **WiFi/Modem** | 第三方 + 开源 | 闭源 firmware，签名分发 |
| **文档** | 中文丰富，社区论坛多 | 全英，Qualcomm 官方文档严谨但偏内部 |
| **改 BSP** | 改了就改了 | 改 BSP 影响过认证，慎改 |

### 学习策略建议

- **M1-M5**：主用 RK3568（学曲线低、社区好），快速建立工程感
- **M5 后半 + M6**：拿 QCS6490 实战，对比体会闭源生态怎么干活
- **M7**：QCS8300 项目时已成熟，能直接介入

### Qualcomm 路线特殊技能（M5-M7 期间）

- 学会读 ATF (ARM Trusted Firmware) 流程
- 学会用 Linaro 工具链
- 看 `meta-qcom` Yocto layer 怎么组织
- 学会 fastboot + edl 烧录工具
- 习惯 SMC 调用、TEE/QSEE 概念
- 闭源驱动逆向：从用户态 ioctl 反推 driver 接口

---

## 10. 资源清单

### 必看（每周）

- [LWN.net 周刊](https://lwn.net/)：每周一次，订阅 $9/月可看新文章，免费用户晚一周
- [bootlin elixir](https://elixir.bootlin.com/linux/latest/source)：在线读源码神器

### 书（按优先级）

| 优先级 | 书 | 用途 |
|-------|-----|------|
| ⭐⭐⭐ | *Linux Device Drivers, 3rd Ed* (LDD3) | 经典，免费在线 (jonesforth.org) |
| ⭐⭐⭐ | *Linux Kernel Development* (LKD) - Robert Love | 内核子系统概览 |
| ⭐⭐ | *Linux Kernel Programming* - Kaiwan Billimoria | 偏现代 (5.x 内核) |
| ⭐⭐ | *Mastering Embedded Linux Programming* | 工程视角，Yocto/Buildroot |
| ⭐ | *Essential Linux Device Drivers* | 老但厚实 |

### 在线训练 / 视频

- [Bootlin Embedded Linux Training](https://bootlin.com/training/) - 培训材料免费下载
- Linux Plumbers Conference（LPC）历年视频
- Embedded Linux Conference（ELC）历年视频
- [Free Electrons / Bootlin YouTube](https://www.youtube.com/@FreeElectrons)

### 邮件列表（订阅，不强求全读）

- `linux-kernel@vger.kernel.org`（巨量，看不完，只看相关）
- `linux-rockchip@lists.infradead.org`（RK 专用）
- `linux-arm-msm@vger.kernel.org`（Qualcomm 专用）
- `linux-media@vger.kernel.org`（V4L2/Camera）

### 工具速查（必装必会）

```text
clangd            代码索引、跳转
ctags / cscope    旧风格但快
ripgrep (rg)      比 grep 快
fzf               模糊查找
tig               git 可视化
htop / btop       系统状态
minicom/picocom   串口
bpftrace          内核追踪
trace-cmd         ftrace 前端
perf              性能采样
```

---

## 11. 进度跟踪与自检

### 进度看板（更新这个表 = 维护进度）

| 阶段 | 时长 | 开始日期 | 完成日期 | 状态 |
|------|------|---------|---------|------|
| M0  教科书阶段 | - | - | 2026-05-16 | ✅ 完成 |
| M1  环境地基 | 2 周 | | | 🔵 进行中 |
| M2  阅读训练营 | 3-4 周 | | | ⏳ |
| M3  修改训练 | 3-4 周 | | | ⏳ |
| M4  调试武器化 | 集中 1 周 | | | ⏳ |
| M5  子系统点亮 | 6-12 周 | | | ⏳ |
| M6  工程实践 | 穿插 | | | ⏳ |
| M7  方向专精 | 长期 | | | ⏳ |

### 阶段自检（完成下一阶段前必答）

**M1 自检**（能上 M2 的标准）：

- [ ] RK3568 串口能稳定通信
- [ ] VM 编出的 .ko 板子能立刻 `insmod`
- [ ] 改一行代码 → 板子看到效果 < 2 分钟
- [ ] 能从 dts 找到对应驱动文件
- [ ] git format-patch 流程能跑通

**M2 自检**：

- [ ] 拿到一个陌生驱动 1 小时内能画出骨架图
- [ ] 能解释每个 case 里 devm_* 为什么这么用
- [ ] mainline vs BSP 对比能说出"vendor 改了什么 / 为什么"
- [ ] 公司代码至少拆过 1 段

**M3 自检**：

- [ ] 能在不破坏现有功能前提下加 sysfs 节点
- [ ] 错误回滚逻辑写得干净（probe 失败不漏资源）
- [ ] 每个修改有清晰的 commit message

**M4 自检**：

- [ ] ftrace 抓过一个真实驱动的执行流
- [ ] bpftrace 写过至少 3 个一行命令
- [ ] 解读过一次完整的 oops 现场
- [ ] kgdb 在 RK3568 上设过断点

**M5 自检**：

- [ ] 至少 3 个子系统能讲清"框架长啥样、关键结构体、典型注册流程"
- [ ] V4L2 + DMA-BUF 至少各跑通一个 demo
- [ ] RKNPU 用户态调用过、看过 ioctl 参数

**M6 自检**：

- [ ] Buildroot 给自己板子做出过完整 rootfs
- [ ] 提过至少 1 个主线 patch（哪怕被 reject 也算）
- [ ] checkpatch.pl 跑过自己所有代码

**M7 自检**：

- [ ] 能向同事讲清 Camera + ISP + NPU 整条链路
- [ ] 能独立 debug 公司项目的真实 bug
- [ ] 能 review 别人的驱动 patch 并提合理建议

---

## 12. 给新 AI 模型的接续提示词

如果开新对话需要接续，把这段复制给新模型：

```text
我在按一份《Linux 驱动工程师学习宪章》学习，文档在我仓库根目录 ROADMAP.md。
请先读这份文档，然后按以下要求协助我：

1. 我是研一在端侧 AI 公司实习的学生，已学完教科书阶段（OSTEP/APUE/cdev 驱动），
   现在学的是"教科书 → 工程能用"的进阶路线。

2. 我有 RK3568（讯为 iTOP）+ Radxa Dragon Q6A（QCS6490）板子，
   未来会做 QCS8300 AI 相机项目。

3. 当前在 M[X] 阶段（请看 ROADMAP §11 进度看板）。

4. 协助原则：
   - 不要回到按 OSTEP/APUE 章节讲的老路
   - 不要堆砌教科书例子
   - 要给"在 RK3568 / QCS6490 上怎么做"的具体步骤
   - 笔记按 ROADMAP §6 仓库结构和 §5.4 案例驱动法组织
   - 严格遵守 §8 的 10 条铁律

5. 我希望的输出风格：直白精简、表格优于段落、带面试题答案、可以直接 commit 进仓库。

具体本次需要的帮助：[在这里说你的问题]
```

---

## 13. 修订历史

| 版本 | 日期 | 修订内容 |
|------|------|---------|
| v1.0 | 2026-05-16 | 初版，定下 M1-M7 路线和方法论 |

---

## 14. 一句话总结

> **教科书让你"知道"，工程让你"做到"。**  
> **"做到"的核心是：读现有代码 80% + 改 15% + 写 5%。**  
> **板子、环境、迭代环、笔记结构、对外贡献——磨好这五把刀，剩下就是时间问题。**