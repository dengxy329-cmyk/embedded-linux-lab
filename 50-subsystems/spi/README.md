# 50-subsystems / spi

| 项 | 值 |
|----|---|
| 优先级 | P1 |
| AI 端侧相关度 | 中（NOR Flash / 高速 sensor） |
| 阶段 | M5 第 5 个 |
| 状态 | ⏳ 未开始 |

## 三件事

1. 框架：`spi_master` / `spi_controller` / `spi_device`
2. 读：`drivers/spi/spi-rockchip.c`（M2 case-04 复用）
3. 改：把一个 SPI sensor 挂到 RK3568 上

## 计划文件

- [ ] `00-framework.md`
- [ ] `01-typical-impl.md`
- [ ] `02-mainline-vs-bsp.md`
- [ ] `03-my-toy.md`             —— 可选
- [ ] `04-interview-q.md`
- [ ] `refs/README.md`

## 关键问题

- 4 种 SPI mode 怎么影响时序
- DMA streaming mapping
- spi_message / spi_transfer 关系
