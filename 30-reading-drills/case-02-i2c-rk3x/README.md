# case-02 / i2c-rk3x

| 项 | 值 |
|----|---|
| mainline 路径 | `drivers/i2c/busses/i2c-rk3x.c` |
| 大小 | ~1500 行 |
| 学什么 | I2C controller、中断驱动、时序、状态机、PM runtime |
| 阶段 | M2 第 2 个 case |
| 状态 | ⏳ 未开始 |

## 计划文件（按 §5.4）

- [ ] `00-overview.md`
- [ ] `01-dts.md`          —— i2c0/i2c1 节点 + clocks/pinctrl 引用
- [ ] `02-walkthrough.md`  —— probe + 状态机
- [ ] `03-trace.md`        —— 一次 `i2c_master_send` 的完整调用链
- [ ] `04-modifications.md` —— M3 填
- [ ] `05-debug-log.md`
- [ ] `refs/README.md`

## elixir 链接

- mainline v6.6: <https://elixir.bootlin.com/linux/v6.6/source/drivers/i2c/busses/i2c-rk3x.c>
- Rockchip BSP: <https://github.com/rockchip-linux/kernel/blob/develop-6.1/drivers/i2c/busses/i2c-rk3x.c>

## 学习重点

- I2C 子系统三件套：`i2c_adapter` / `i2c_algorithm` / `i2c_msg`
- 中断处理：top-half 直接 in IRQ，怎么处理多 msg 续传
- DMA vs PIO：什么时候走 DMA，FIFO 阈值
- 100k / 400k / 1M 时钟分频怎么算
