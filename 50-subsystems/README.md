# 50-subsystems / 子系统点亮（M5 长期，6-12 周）

> 按 AI 端侧依赖链点亮：GPIO → Pinctrl → Clock → I2C/SPI → IIO → **V4L2 + DMA-BUF** → DRM → NPU。
> 不必每个都做"完"，按需点亮；AI 端侧最重要的是 V4L2 + DMA-BUF + IOMMU，到这一步**要重锤砸**。

## 这个目录干啥

每个子系统三件事（每个目录的统一结构）：
1. 画框架图（核心结构体、注册流程、调用关系）
2. 读一个典型实现（mainline + BSP 对比）
3. 自己改一个或写一个简化版

## 子系统清单（按依赖链顺序）

| # | 目录 | 优先级 | AI 端侧相关度 | 典型 RK 驱动 | 典型 QCS 驱动 |
|---|------|--------|--------------|------------|--------------|
| 1 | `gpio/` | P0 | 低（但是模板） | gpio-rockchip | gpio-msm |
| 2 | `pinctrl/` | P0 | 低 | pinctrl-rockchip | pinctrl-msm |
| 3 | `clk-and-power/` | P0 | 中 | clk-rk3568 | clk-qcom |
| 4 | `i2c-and-iio/` | P1 | 中（sensor） | i2c-rk3x + iio bmi160 | i2c-qcom-geni |
| 5 | `spi/` | P1 | 中 | spi-rockchip | spi-geni-qcom |
| 6 | `v4l2-camera/` ⭐ | **P0** | **高（Camera 核心）** | rkisp1 | qcom camss（部分） |
| 7 | `dma-buf/` ⭐ | **P0** | **高（NPU 内存核心）** | dma-buf 通用 | dma-buf 通用 |
| 8 | `iommu/` | P1 | 高（AI 加速器） | rockchip iommu | arm-smmu-qcom |
| 9 | `drm-display/` | P2 | 低 | rockchip drm | msm drm |
| 10 | `rknpu-research/` | P1 | **高（闭源逆向）** | rknpu（部分闭源） | - |
| 11 | `qcom-hexagon-research/` | P2 | 高（QCS8300 长线） | - | fastrpc + hexagon |

## AI 端侧路线重点 ⭐⭐⭐

**V4L2 + DMA-BUF + IOMMU** = 端侧 AI 视觉的三件套，按 ROADMAP §4 M5 要重锤砸。

```text
sensor → MIPI CSI → ISP → buffer → NPU → 输出
         (V4L2 subdev)   (DMA-BUF)  (IOMMU 隔离)
```

## 每个子系统笔记模板

```text
xxx/
├── README.md            框架图 + 关键结构体 + 注册流程
├── 00-framework.md      子系统总览（kernel 文档里学不到的工程视角）
├── 01-typical-impl.md   挑一个典型驱动精读
├── 02-mainline-vs-bsp.md  vendor 改了什么、为什么
├── 03-my-toy.md         自己写的简化版（可选，复杂子系统不强求）
├── 04-interview-q.md    面试高频题 + 答案
└── refs/                LWN 文章、关键 commit、mailing list 讨论
```

## 推荐学习节奏

| 周 | 子系统 | 输出 |
|----|--------|------|
| W1 | gpio + pinctrl | 2 篇 framework + 1 篇对比 |
| W2 | clk + power | 1 篇 framework + 时钟树画图 |
| W3-4 | i2c + iio | 1 个完整 case（sensor 挂上去） |
| W5 | spi | 框架 + 对比 |
| W6-8 | **v4l2-camera** ⭐ | rkisp1 + 一个 camera pipeline 跑通 |
| W9-10 | **dma-buf + iommu** ⭐ | 共享 buffer demo（V4L2 → ION/dma-heap → NPU 模拟） |
| W11 | drm（按需） | - |
| W12 | rknpu / qcs hexagon | ioctl 逆向笔记 |

## 闭源驱动的方法（§9 关键技能）

- **看用户态 SDK 调用反推 driver 接口**
- strace 抓 ioctl 序号 + 参数结构
- 在 SDK 源码里搜 `ioctl(` 调用点，反查 driver 端的 `case XXX_IOCTL_YYY`
- 关键 ioctl 必抓：alloc / map / submit / wait / free

## M5 自检（ROADMAP §11）

- [ ] 至少 3 个子系统能讲清"框架长啥样、关键结构体、典型注册流程"
- [ ] V4L2 + DMA-BUF 至少各跑通一个 demo
- [ ] RKNPU 用户态调用过、看过 ioctl 参数
