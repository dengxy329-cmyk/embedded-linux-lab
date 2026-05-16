# 50-subsystems / i2c-and-iio

| 项 | 值 |
|----|---|
| 优先级 | P1（sensor 路线必备） |
| AI 端侧相关度 | 中（Camera 模组里的 EEPROM / VCM / IMU 都走 I2C） |
| 阶段 | M5 第 4 个 |
| 状态 | ⏳ 未开始 |

## 三件事

1. 框架：I2C `i2c_adapter` + IIO `iio_dev` 双子系统
2. 读：`drivers/i2c/busses/i2c-rk3x.c`（M2 case-02 复用） + `drivers/iio/imu/bmi160/`（M2 case-03 复用）
3. 改：把一个新 sensor 挂到 RK3568 I2C 总线上跑通

## 计划文件

- [ ] `00-framework.md`
- [ ] `01-typical-impl.md`
- [ ] `02-mainline-vs-bsp.md`
- [ ] `03-my-toy.md`             —— 写一个假 sensor 走 I2C
- [ ] `04-interview-q.md`
- [ ] `refs/README.md`

## 关键问题

- regmap 怎么屏蔽 I2C / SPI 差异
- IIO triggered buffer 模型
- I2C 时序问题怎么用示波器排
