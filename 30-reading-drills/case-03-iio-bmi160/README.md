# case-03 / iio-bmi160

| 项 | 值 |
|----|---|
| mainline 路径 | `drivers/iio/imu/bmi160/` |
| 大小 | ~1500 行（多文件） |
| 学什么 | IIO 子系统、sysfs / chardev、triggered buffer、regmap |
| 阶段 | M2 第 3 个 case |
| 状态 | ⏳ 未开始 |

## 计划文件（按 §5.4）

- [ ] `00-overview.md`
- [ ] `01-dts.md`          —— I2C 或 SPI 节点
- [ ] `02-walkthrough.md`  —— core/i2c/spi 三个文件如何分工
- [ ] `03-trace.md`        —— `cat /sys/bus/iio/devices/iio:device0/in_accel_x_raw` 走到哪
- [ ] `04-modifications.md` —— M3 填（候选：挂到 RK3568 I2C2）
- [ ] `05-debug-log.md`
- [ ] `refs/README.md`

## elixir 链接

- mainline v6.6: <https://elixir.bootlin.com/linux/v6.6/source/drivers/iio/imu/bmi160>
- IIO 子系统文档: <https://elixir.bootlin.com/linux/v6.6/source/Documentation/iio>

## 学习重点

- IIO 是"传感器子系统"，所有 sensor 都走这个框架
- regmap：屏蔽 I2C / SPI 差异，写一次 driver 跑两种总线
- triggered buffer：硬件中断/timer 触发，避免 polling
- sysfs ABI：稳定接口，**铁律 §8.8 永不破坏**
