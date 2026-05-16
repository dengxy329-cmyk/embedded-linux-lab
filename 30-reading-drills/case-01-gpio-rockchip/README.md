# case-01 / gpio-rockchip

| 项 | 值 |
|----|---|
| mainline 路径 | `drivers/gpio/gpio-rockchip.c` |
| 大小 | ~600 行 |
| 学什么 | platform driver 骨架、of_match、devm_*、gpiochip |
| 阶段 | M2 第 1 个 case（入门） |
| 状态 | ⏳ 未开始 |

## 计划文件（按 §5.4）

- [ ] `00-overview.md`     —— 这个驱动干啥、为什么挑它
- [ ] `01-dts.md`          —— RK3568 dts 里 gpio 节点 + bindings YAML
- [ ] `02-walkthrough.md`  —— probe 逐段解析（不是逐行抄）
- [ ] `03-trace.md`        —— `gpio_set_value(N, 1)` 从用户态追到寄存器
- [ ] `04-modifications.md` —— M3 填
- [ ] `05-debug-log.md`    —— 踩坑记录
- [ ] `refs/README.md`     —— 参考过的 commit / LWN / 邮件

## elixir 链接

- mainline v6.6: <https://elixir.bootlin.com/linux/v6.6/source/drivers/gpio/gpio-rockchip.c>
- Rockchip BSP: <https://github.com/rockchip-linux/kernel/blob/develop-6.1/drivers/gpio/gpio-rockchip.c>
- dt-bindings: <https://elixir.bootlin.com/linux/v6.6/source/Documentation/devicetree/bindings/gpio/rockchip,gpio-bank.yaml>
