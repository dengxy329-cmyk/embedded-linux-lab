# 50-subsystems / remoteproc

| 项 | 值 |
|----|---|
| 优先级 | P0（QCS8300 项目时变 ⭐⭐⭐） |
| AI 端侧相关度 | **高**（Hexagon DSP/NPU 启动控制） |
| 阶段 | M5 后期 |
| 状态 | ⏳ 未开始 |

## 这个子系统干啥

控制带独立 CPU 的协处理器（Hexagon DSP、Cortex-M、MCU、modem）的启动、停止、firmware 加载、状态机管理。
端侧 AI 路线上是 **Hexagon NPU 的基础设施**——QCS8300 项目绕不开。

## 三件事

1. 框架：`rproc_ops` / `rproc_subdev` / firmware 加载流程 / 状态机
2. 读：`drivers/remoteproc/qcom_q6v5_*.c` + `drivers/remoteproc/remoteproc_core.c`
3. 跑：在 Q6A 上看 fastrpc 怎么和 remoteproc 配合

## 计划文件

- [ ] `00-framework.md`              —— rproc 子系统总览、状态机图
- [ ] `01-qcom-q6v5.md`              —— Qualcomm Hexagon driver 实战
- [ ] `02-firmware-loading.md`       —— `.mdt`/`.b00`/`.b01` 文件格式、签名
- [ ] `03-rproc-and-fastrpc.md`      —— remoteproc + fastrpc 协作
- [ ] `04-interview-q.md`
- [ ] `refs/README.md`

## 关键问题（M5 必答）

- remoteproc 怎么 attach 一个独立处理器（PIL、Q6V5、加载 → 启动 → handover）
- firmware 文件分包格式（mdt 头 + bN 段）
- 启动顺序：power on → reset → load firmware → release reset → handshake
- 与 IOMMU 的配合（DSP 看到的地址 = 主 CPU 看到的物理地址？）
- 与 RPMSG 的关系（双向消息通道，建立在 remoteproc 之上）
- 与 PD (Protection Domain) 的关系（Qualcomm 特有）

## 与 QCS8300 项目的关系

QCS8300 上 Hexagon NPU 就是一个 remote processor。
理解 remoteproc = 看懂 QCS 端侧 AI 半个软件栈。

## 与隔壁子系统的关系

| 子系统 | 关系 |
|--------|------|
| `qcom-hexagon-research/` | 这里管"加载启动"，那里管"运行时调用" |
| `iommu/` | DSP 访问内存必经 IOMMU |
| `dma-buf/` | DSP 处理的 buffer 来自 DMA-BUF |

## elixir 链接

- mainline v6.6: <https://elixir.bootlin.com/linux/v6.6/source/drivers/remoteproc>
- Documentation: <https://elixir.bootlin.com/linux/v6.6/source/Documentation/staging/remoteproc.rst>
