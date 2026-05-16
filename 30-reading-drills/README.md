# 30-reading-drills / 阅读训练营（M2 主战场）⭐⭐⭐

> **M2 期间不写新代码、只读 + 笔记**。这是 ROADMAP §4 M2 的反直觉规则。
> 工程师 60% 时间在读现有代码，先把这件事训练出来。

## 这个目录干啥

按 §5.1 七步法读 8 个真实驱动，每个一个子目录，按 §5.4 案例驱动法组织。

## 每个 case 的目录结构（铁律）

```text
case-NN-xxx/
├── 00-overview.md       这个驱动干啥、为什么挑它、硬件背景
├── 01-dts.md            dts 节点 + 对应 bindings YAML
├── 02-walkthrough.md    probe 逐段解析（不是逐行抄）
├── 03-trace.md          一次操作的完整调用栈（用户态 → 硬件寄存器）
├── 04-modifications.md  我对它做了什么、为什么（M3 才填）
├── 05-debug-log.md      碰到的坑、怎么排的（war story）
└── refs/                参考过的 commits / LWN / 邮件
    └── README.md        每条引用一行简介
```

## Case 清单（由浅入深）

| # | Case | mainline 路径 | 大小 | 学什么 | 状态 |
|---|------|--------------|------|--------|------|
| 01 | gpio-rockchip | `drivers/gpio/gpio-rockchip.c` | ~600 行 | platform driver 骨架、of_match、devm_* | ⏳ |
| 02 | i2c-rk3x | `drivers/i2c/busses/i2c-rk3x.c` | ~1500 行 | I2C controller、中断、时序、状态机 | ⏳ |
| 03 | iio-bmi160 | `drivers/iio/imu/bmi160/` | ~1500 行 | 子系统驱动（IIO）、sysfs、触发器 | ⏳ |
| 04 | spi-rockchip | `drivers/spi/spi-rockchip.c` | ~1200 行 | SPI controller、DMA、FIFO | ⏳ |
| 05 | pwm-rockchip | `drivers/pwm/pwm-rockchip.c` | ~500 行 | 简单子系统完整例子 | ⏳ |
| 06 | rkisp1-camera | `drivers/media/platform/rockchip/rkisp1/` | 大 | Camera/ISP（AI 方向核心） | ⏳ |
| 07 | internal-xxx | 公司代码（脱敏） | - | 实战拆解 | ⏳ |
| 08 | qcs6490-yyy | Qualcomm 某简单驱动 | - | 对比 Rockchip 风格 | ⏳ |

## 每个 case 节奏（参考 1.5-2 天 / case）

| 阶段 | 内容 | 耗时 |
|------|------|------|
| 1 | 通读 dts + 找 compatible → 找 driver 文件 | 1 小时 |
| 2 | 读 probe 函数（用 §5.1 七步法） | 2-3 小时 |
| 3 | 挑一个 read/write/ioctl 全栈追到寄存器 | 2-3 小时 |
| 4 | 画 mermaid 调用图 | 1-2 小时 |
| 5 | mainline vs BSP 对比（git log + diff） | 1-2 小时 |
| 6 | 写 5 篇笔记（00-05） | 半天 |

**8 个 case ≈ 2-3 周**（每周 20 小时）。

## 禁忌（铁律 §8）

- ❌ 不要在 M2 期间动手改代码（M3 才改）
- ❌ 不要从第一行读到最后一行（§5.1 禁令：源码是图，不是书）
- ❌ 不要只看 mainline 不看 BSP（vendor 改了什么、为什么 = M2 核心知识）
- ✅ 每个 case 必须画出调用图
- ✅ 每个 case 必须追一次完整调用栈

## M2 自检（ROADMAP §11）

- [ ] 拿到一个陌生驱动 1 小时内能画出骨架图
- [ ] 能解释每个 case 里 devm_* 为什么这么用
- [ ] mainline vs BSP 对比能说出"vendor 改了什么 / 为什么"
- [ ] 公司代码至少拆过 1 段
