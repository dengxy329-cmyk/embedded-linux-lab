# 50-subsystems / v4l2-camera ⭐⭐⭐

| 项 | 值 |
|----|---|
| 优先级 | **P0**（端侧 AI Camera 核心） |
| AI 端侧相关度 | **高** |
| 阶段 | M5 第 6 个（**重锤砸**） |
| 状态 | ⏳ 未开始 |

## 三件事（这个子系统按 ROADMAP §4 要砸 2-3 周）

1. 框架：V4L2 子系统全图 —— device / subdev / video_device / media controller
2. 读：rkisp1（M2 case-06 复用） + 一个 sensor driver（ov 系列） + 一个 csi driver
3. 改/写：跑通一个 Camera pipeline（sensor → CSI → ISP → /dev/video0 出图）

## 计划文件（这个子系统会比其他多）

- [ ] `00-framework.md`             —— V4L2 全景图
- [ ] `01-subdev-model.md`          —— subdev 怎么注册、graph 怎么连
- [ ] `02-media-controller.md`      —— media-ctl 工具用法
- [ ] `03-videobuf2.md`             —— vb2 + DMA-BUF 配合
- [ ] `04-rkisp1-pipeline.md`       —— 跑通 RK3568 上的 capture
- [ ] `05-mipi-csi.md`              —— MIPI CSI 协议、lane / clock
- [ ] `06-sensor-driver-template.md` —— 怎么写一个 sensor driver
- [ ] `07-interview-q.md`
- [ ] `refs/README.md`

## 工具链

```text
v4l2-ctl       看 video device 能力
media-ctl      看/改 media graph
yavta          抓帧测试
v4l-utils      Yocto/Buildroot 都有
gstreamer      测高级 pipeline
```

## 关键问题

- subdev pad 是什么？source / sink
- format 怎么从 sensor 一路传播到 video device
- 一个 buffer 从 user → vb2 → DMA → ISP → 写回 → DQBUF 的完整生命周期
- 3A（AE/AWB/AF）userspace daemon 怎么和 driver 交互（params/stats node）

## 与 M7 的关系

M7 AI Camera 专精的基础全在这里。M5 这一节做不好 → M7 做不动。
