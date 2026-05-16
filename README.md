# embedded-linux-lab

> 工程导向的 Linux 驱动 / BSP 学习笔记 — 在 Rockchip RK3568 与 Qualcomm QCS6490 上动手实践

## 📍 关于这个仓库

教科书阶段的笔记在另一个仓库：[linux-kernel-learning](https://github.com/dengxy329-cmyk/linux-kernel-learning)（OSTEP / APUE / 教科书 cdev 驱动）。

**这个仓库面向的是下一步**——从"会写 hello world 驱动"到"能在 vendor BSP 里干活"。

| 文档 | 用途 |
|------|------|
| [📖 ROADMAP.md](./ROADMAP.md) | 学习宪章：路线、方法论、进度看板、铁律 |
| [🤖 AGENTS.md](./AGENTS.md) | 接续协助的 AI 必读：上下文 + 协助原则 + 笔记落点 |
| [🎨 STYLE.md](./STYLE.md) | 笔记风格手册：语言/链接/图表/commit message 约定 |

**骨架已就位**：11 个一级目录 + M2 8 个 case 子目录 + M5 12 个子系统子目录 + 各类 templates。
打开任何子目录的 `README.md` 都能看到"该填什么、何时填、参考链接"。

## 🛠️ 学习硬件

| 板子 | 用途 | 阶段 |
|------|------|------|
| 讯为 iTOP-RK3568 | 主学习板 | M1-M6 |
| Radxa Dragon Q6A (QCS6490) | AI 端侧实战 | M5-M7 |
| Qualcomm QCS8300 (项目) | 车载 AI 相机 | M7 |

## 🗂️ 仓库结构

```text
00-env/                环境地基（VM、跨编译、NFS root、串口）
10-fundamentals/       内核基础速查（不是教程，是参考）
20-driver-model/       平台驱动 / 设备树 / probe 模式
30-reading-drills/     ⭐ 真实驱动阅读案例（M2 主战场）
40-modification-drills/ 修改训练
50-subsystems/         子系统按需点亮（GPIO / I2C / V4L2 / DMA-BUF ...）
60-debug-arsenal/      调试武器库（ftrace / bpftrace / kgdb / perf）
70-build-systems/      Kbuild / Buildroot / Yocto / U-Boot
80-patches/            对外贡献（mainline patch）
90-interview-prep/     面试题与系统设计
99-resources/          书签、命令速查、链接
```

## 📊 进度看板

| 阶段 | 时长 | 状态 | 进展 |
|------|------|------|------|
| M1 环境地基 | 2 周 | 🔵 进行中 | 骨架已建，等待 D1-D14 填内容 |
| M2 阅读训练营 | 3-4 周 | ⏳ | 8 个 case 入口已就位 |
| M3 修改训练 | 3-4 周 | ⏳ | - |
| M4 调试武器化 | 集中 1 周 | ⏳ | - |
| M5 子系统点亮 | 6-12 周 | ⏳ | 12 个子系统入口已就位（含 remoteproc） |
| M6 工程实践 | 穿插 | ⏳ | - |
| M7 方向专精（AI Camera/ISP/NPU） | 长期 | ⏳ | - |

## 🎯 核心理念

- **读 > 改 > 调 > 写**（工程师 60% 时间在读现有代码）
- **mainline + BSP 双线对比**（看 vendor 改了什么、为什么）
- **每个案例都跑在真硬件上**（QEMU 通过 ≠ 真板能跑）
- **永远 devm_*** / 永远写好 commit message / 永不破坏用户态 ABI

## 📚 参考

- [bootlin elixir](https://elixir.bootlin.com/linux/v6.6/source) — 在线读源码
- [LWN.net](https://lwn.net/) — 内核周刊
- Linux v6.6 LTS — 主学习版本
- Rockchip BSP: <https://github.com/rockchip-linux/kernel>
- Qualcomm Linaro: <https://git.codelinaro.org/clo/la/kernel>

## License

MIT