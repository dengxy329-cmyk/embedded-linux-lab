# 50-subsystems / clk-and-power

| 项 | 值 |
|----|---|
| 优先级 | P0（每个外设都依赖时钟和电源） |
| AI 端侧相关度 | 中（Camera/NPU 都要管功耗） |
| 阶段 | M5 第 3 个 |
| 状态 | ⏳ 未开始 |

## 三件事

1. 框架：`clk_hw` / `clk_provider` / `clk_consumer`，regulator 同理
2. 读：`drivers/clk/rockchip/clk-rk3568.c`（庞大，挑一个 clk_branch 看）
3. 改：把一个外设的 clock_rate 从 dts 配出来

## 计划文件

- [ ] `00-framework.md`         —— Common Clock Framework (CCF) 总览
- [ ] `01-typical-impl.md`
- [ ] `02-mainline-vs-bsp.md`
- [ ] `03-my-toy.md`             —— 可选
- [ ] `04-interview-q.md`
- [ ] `refs/README.md`

## 关键问题

- `devm_clk_get` vs `devm_clk_get_optional` 何时用
- `clk_prepare_enable` 为什么分两步
- runtime PM 和时钟的关系
- regulator 的电压协商
- AI 加速器的 power domain（genpd）
