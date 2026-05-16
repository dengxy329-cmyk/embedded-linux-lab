# 50-subsystems / gpio

| 项 | 值 |
|----|---|
| 优先级 | P0（最基础，是其他子系统的模板） |
| AI 端侧相关度 | 低（但是入门） |
| 阶段 | M5 第 1 个 |
| 状态 | ⏳ 未开始 |

## 三件事

1. 画框架图：`gpio_chip` / `gpio_desc` / sysfs 接口
2. 读典型实现：mainline `gpio-rockchip.c`（M2 case-01 复用）
3. 自己改：给某个引脚加 sysfs 节点

## 计划文件

- [ ] `00-framework.md`
- [ ] `01-typical-impl.md`     —— 复用 case-01 的笔记 + 补 framework 视角
- [ ] `02-mainline-vs-bsp.md`
- [ ] `03-my-toy.md`           —— 写一个 fake-gpio 驱动
- [ ] `04-interview-q.md`
- [ ] `refs/README.md`
