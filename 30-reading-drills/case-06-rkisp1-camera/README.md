# case-06 / rkisp1-camera ⭐⭐⭐

| 项 | 值 |
|----|---|
| mainline 路径 | `drivers/media/platform/rockchip/rkisp1/` |
| 大小 | 大（多文件，10+） |
| 学什么 | V4L2 框架、subdev、media controller、ISP pipeline |
| 阶段 | M2 第 6 个 case（**AI Camera 方向核心**） |
| 状态 | ⏳ 未开始 |

## 计划文件（按 §5.4）

- [ ] `00-overview.md`     —— RK ISP1 硬件框图 + driver 文件分工
- [ ] `01-dts.md`          —— isp + csi + sensor 节点的 graph 绑定
- [ ] `02-walkthrough.md`  —— probe + capture/params/stats 三个 video device
- [ ] `03-trace.md`        —— 一帧从 sensor → CSI → ISP → DMA → 用户态 buffer
- [ ] `04-modifications.md` —— M3/M5 填
- [ ] `05-debug-log.md`
- [ ] `refs/README.md`

## elixir 链接

- mainline v6.6: <https://elixir.bootlin.com/linux/v6.6/source/drivers/media/platform/rockchip/rkisp1>
- V4L2 docs: <https://elixir.bootlin.com/linux/v6.6/source/Documentation/userspace-api/media/v4l>

## 学习重点（M5 v4l2-camera 子系统的源料）

- V4L2 subdev 模型（isp / csi / sensor 各是一个 subdev）
- media controller graph
- videobuf2 + DMA-BUF
- ISP 三个 video node：capture / params (3A 输入) / stats (3A 输出)
- userspace 配合的 daemon：rkaiq / yavta / v4l2-ctl

## 与 ROADMAP §4 M7 的关系

这个 case 是从 M2 直通 M7 AI Camera 方向的**最关键入口**。
M2 看个大概，M5 重锤砸，M7 专精。
