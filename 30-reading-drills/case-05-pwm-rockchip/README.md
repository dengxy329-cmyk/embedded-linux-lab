# case-05 / pwm-rockchip

| 项 | 值 |
|----|---|
| mainline 路径 | `drivers/pwm/pwm-rockchip.c` |
| 大小 | ~500 行 |
| 学什么 | 简单子系统的完整例子（pwm_chip / pwm_ops） |
| 阶段 | M2 第 5 个 case（轻松一篇） |
| 状态 | ⏳ 未开始 |

## 计划文件（按 §5.4）

- [ ] `00-overview.md`
- [ ] `01-dts.md`          —— pwm 节点
- [ ] `02-walkthrough.md`  —— probe + pwm_ops（apply / get_state）
- [ ] `03-trace.md`        —— pwm 用户态 sysfs 接口走到哪
- [ ] `04-modifications.md` —— M3 填
- [ ] `05-debug-log.md`
- [ ] `refs/README.md`

## elixir 链接

- mainline v6.6: <https://elixir.bootlin.com/linux/v6.6/source/drivers/pwm/pwm-rockchip.c>

## 学习重点

- 子系统接入模板的"最小可读版"
- 周期/占空比寄存器计算
- atomic pwm API（新接口）
