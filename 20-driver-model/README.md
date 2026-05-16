# 20-driver-model / 驱动模型（M2 前置薄知识）

> 读 case 前把"platform driver / 设备树 / probe 失败回滚 / sysfs" 4 张图刻在脑子里。
> **总共 2 天读完 + 1 天画图**，读完立刻开 M2 case-01，不要在这里停留。

## 这个目录干啥

M2 case study 之前必须先建立的 4 个心智模型。
不读完这 4 篇就去读 `gpio-rockchip.c` 会看不懂注册流程为什么这么写。

## 文件清单

| 文件 | 必须回答的关键问题 | 卡多少时间 |
|------|---------|----------|
| `platform-driver.md` | `platform_driver_register` 背后发生了什么？bus_type 在哪？为什么不用字符设备直接 register？ | 半天 |
| `of-and-bindings.md` | `of_match_table` 怎么匹配 `compatible`？dt-bindings YAML 怎么读？dts 的 `reg`/`interrupts`/`clocks` 怎么变成 C 里的资源？ | 半天 |
| `probe-failure-and-rollback.md` | probe 返回 `-EPROBE_DEFER` 内核会怎么做？devm_* 帮你做了什么？probe 失败的资源怎么释放？ | 半天 |
| `sysfs-debugfs.md` | `DEVICE_ATTR` / `DEFINE_SHOW_ATTRIBUTE` 选哪个？sysfs vs debugfs vs procfs 用法分界？ | 半天 |

## 不写什么

- ❌ 不写"什么是字符设备"——M0 已会
- ❌ 不写 `file_operations` 全表——case 里读到会自然学
- ❌ 不写 LDD3 第几章 —— 章节笔记在 [linux-kernel-learning](https://github.com/dengxy329-cmyk/linux-kernel-learning)

## 写什么

每篇必须包含：
1. **一张框架图**（mermaid 或 ASCII）
2. **一段真实代码**（mainline，带文件路径和行号）
3. **3-5 道面试题 + 答案要点**
4. **一段"我曾经搞错的地方"**

## 与 ROADMAP §1 痛点的对应

> "看公司真实代码懵——平台驱动、子系统、devm_*、设备树串起来理不清"

这个目录就是治这个的。

| 痛点 | 对应文件 |
|------|---------|
| 平台驱动 | `platform-driver.md` |
| 设备树 | `of-and-bindings.md` |
| devm_* | `probe-failure-and-rollback.md` + `10-fundamentals/modern-patterns.md` |
| 子系统怎么串 | M2 case study（这里只打底） |
