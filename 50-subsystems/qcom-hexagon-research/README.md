# 50-subsystems / qcom-hexagon-research（闭源逆向）

| 项 | 值 |
|----|---|
| 优先级 | P2（QCS8300 项目时变 P0） |
| AI 端侧相关度 | **高**（Qualcomm 端侧 AI 路线核心） |
| 阶段 | M5 第 11 个（长线） |
| 状态 | ⏳ 未开始 |

## 这个目录干啥

Qualcomm Hexagon DSP/NPU **完全闭源**，driver 通过 `fastrpc` 子系统 + 签名 firmware 接入。
按 ROADMAP §9 Qualcomm 路线特殊技能：用户态 SDK + ioctl 反推 + ATF/TEE 调用观察。

## 计划文件

- [ ] `00-hexagon-overview.md`      —— Hexagon DSP/NPU 硬件 + 软件栈
- [ ] `01-fastrpc-subsystem.md`     —— `drivers/misc/fastrpc.c` 框架
- [ ] `02-qnn-sdk.md`               —— Qualcomm Neural Network SDK
- [ ] `03-firmware-signing.md`      —— 签名 firmware 加载流程
- [ ] `04-smc-and-tee.md`           —— TEE 调用观察
- [ ] `05-interview-q.md`
- [ ] `refs/README.md`

## 关键认知

- `fastrpc` 是 mainline 子系统（可读源码）
- 上面跑的 Hexagon firmware 是闭源签名 blob
- userspace 通过 `/dev/adsprpc-smd` 之类节点接入
- 所有 AI 推理 = userspace → fastrpc → Hexagon firmware → 硬件

## 与 QCS8300 项目的关系

QCS8300 车载 AI 相机项目 = Spectra ISP + Hexagon NPU 的全套。
M7 介入前必须先把 fastrpc / TEE / 签名启动 / Yocto meta-qcom 这些 Qualcomm 特性建立基础认知。

## 资源

- mainline: `drivers/misc/fastrpc.c`
- mainline: `drivers/remoteproc/qcom_*`
- CodeLinaro: <https://git.codelinaro.org/clo/la/kernel>
- Qualcomm 官方文档（要注册）
