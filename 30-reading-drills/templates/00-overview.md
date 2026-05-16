# {Case 标题} / overview

| 项 | 值 |
|----|---|
| 驱动路径（mainline） | `drivers/.../xxx.c` |
| 驱动路径（BSP） | `drivers/.../xxx.c`（vendor 分支） |
| 关联硬件 | {芯片型号 / 外设型号} |
| 总线类型 | platform / i2c / spi / pci / ... |
| 子系统归属 | gpio / i2c / iio / v4l2 / ... |
| 学时投入 | {预计 X 天 / 实际 Y 天} |
| 难度 | ★~★★★★★ |
| 开始日期 | YYYY-MM-DD |
| 完成日期 | YYYY-MM-DD |

## 为什么挑这个 case

{2-3 句话，工程视角。不要写"为了学习 platform driver"这种套话，写"因为公司项目 X 用了类似驱动" 或 "因为是 RK3568 上某个外设的入口"。}

## 硬件背景

- 这个外设干啥：
- 在系统里负责什么：
- 典型用法：
- datasheet 链接（如果公开）：

## 软件全景图

```mermaid
graph TD
  A[用户态程序] --> B[/dev/xxx 或 sysfs]
  B --> C[本驱动]
  C --> D[硬件寄存器]
```

## 我想从这个 case 学到什么

- [ ] {学习目标 1，必须可验证}
- [ ] {学习目标 2}
- [ ] {学习目标 3}

## 参考资料

| 类型 | 链接 |
|------|------|
| mainline 源码 | <https://elixir.bootlin.com/linux/v6.6/source/drivers/...> |
| BSP 源码 | <https://github.com/rockchip-linux/kernel/blob/develop-6.1/drivers/...> |
| dt-bindings | <https://elixir.bootlin.com/linux/v6.6/source/Documentation/devicetree/bindings/...> |
| LWN 文章 | {如果有} |
| LKML 讨论 | {如果有} |
| Vendor 文档 | {如果有} |

## 关键发现一句话总结（读完时填）

> {用一句话回答："这个驱动告诉了我什么我之前不知道的事？"}
