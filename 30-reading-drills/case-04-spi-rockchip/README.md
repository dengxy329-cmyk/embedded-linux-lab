# case-04 / spi-rockchip

| 项 | 值 |
|----|---|
| mainline 路径 | `drivers/spi/spi-rockchip.c` |
| 大小 | ~1200 行 |
| 学什么 | SPI controller、DMA streaming、FIFO 水位、CS 控制 |
| 阶段 | M2 第 4 个 case |
| 状态 | ⏳ 未开始 |

## 计划文件（按 §5.4）

- [ ] `00-overview.md`
- [ ] `01-dts.md`          —— spi 节点 + dma-names |
- [ ] `02-walkthrough.md`  —— probe + transfer_one_message
- [ ] `03-trace.md`        —— 一次 spi_sync 的 DMA 路径
- [ ] `04-modifications.md` —— M3 填
- [ ] `05-debug-log.md`
- [ ] `refs/README.md`

## elixir 链接

- mainline v6.6: <https://elixir.bootlin.com/linux/v6.6/source/drivers/spi/spi-rockchip.c>
- Rockchip BSP: <https://github.com/rockchip-linux/kernel/blob/develop-6.1/drivers/spi/spi-rockchip.c>

## 学习重点

- DMA streaming mapping (`dma_map_single` / `dma_unmap_single`)
- cache 维护（Arm v8 上 DMA 和 CPU cache 一致性）
- FIFO 阈值与中断 vs DMA 选择
