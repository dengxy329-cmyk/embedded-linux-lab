# 50-subsystems / pinctrl

| 项 | 值 |
|----|---|
| 优先级 | P0（GPIO 的搭档，dts 必备） |
| AI 端侧相关度 | 低 |
| 阶段 | M5 第 2 个 |
| 状态 | ⏳ 未开始 |

## 三件事

1. 框架：`pinctrl_dev` / `pinctrl_state` / dts `pinctrl-0` 引用
2. 读：`drivers/pinctrl/pinctrl-rockchip.c`
3. 改：给一个 sensor 节点写完整 pinctrl-default

## 计划文件

- [ ] `00-framework.md`
- [ ] `01-typical-impl.md`
- [ ] `02-mainline-vs-bsp.md`
- [ ] `03-my-toy.md`           —— 可选
- [ ] `04-interview-q.md`
- [ ] `refs/README.md`

## 关键问题

- pinctrl 状态机：default / sleep / idle 怎么切？
- 谁触发切换？runtime PM？
- pinctrl 和 GPIO 的边界在哪？（同一个 SoC 引脚，谁先谁后）
