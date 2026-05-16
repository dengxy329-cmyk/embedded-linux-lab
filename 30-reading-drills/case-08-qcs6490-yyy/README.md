# case-08 / qcs6490-yyy（Qualcomm 风格对比）

| 项 | 值 |
|----|---|
| 板子 | Radxa Dragon Q6A (QCS6490) |
| 学什么 | 对比 Rockchip 风格、感受 Qualcomm 工程生态 |
| 阶段 | M2 第 8 个 case |
| 状态 | ⏳ 未开始 |

## 候选 case（任选一个简单的）

| 候选 | 路径 | 难度 |
|------|------|------|
| Geni I2C | `drivers/i2c/busses/i2c-qcom-geni.c` | ★★ |
| Geni SPI | `drivers/spi/spi-geni-qcom.c` | ★★ |
| pinctrl-msm | `drivers/pinctrl/qcom/pinctrl-msm.c` | ★★★ |
| qcom rpmh-rsc | `drivers/soc/qcom/rpmh-rsc.c` | ★★★ |

## 计划文件（按 §5.4）

- [ ] `00-overview.md`     —— Qualcomm SoC 风格特点
- [ ] `01-dts.md`          —— Q6A 设备树（注意 mainline vs CodeLinaro 差异）
- [ ] `02-walkthrough.md`  —— probe + 关键 ops
- [ ] `03-trace.md`        —— 一次操作的调用链
- [ ] `04-modifications.md` —— M3 填（可能不做）
- [ ] `05-debug-log.md`
- [ ] `refs/README.md`

## 与 §9 工程差异表的对应

读这个 case 时重点观察：
- TrustZone / TEE 是否有调用（SMC instruction）
- Qualcomm 厂商 patch 风格（在 mainline driver 上又叠了多少）
- 文档语言（全英、官方文档与社区文档差距）
- 构建系统（如果用 Yocto / meta-qcom）

## elixir 链接

- mainline v6.6: <https://elixir.bootlin.com/linux/v6.6/source/drivers/i2c/busses/i2c-qcom-geni.c>
- CodeLinaro kernel: <https://git.codelinaro.org/clo/la/kernel>
