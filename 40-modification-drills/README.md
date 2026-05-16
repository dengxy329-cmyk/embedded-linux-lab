# 40-modification-drills / 修改训练（M3）

> 在 M2 读过的驱动上做"外科手术"。
> 每个修改一个 commit + 一篇"我改了什么、为什么"的笔记。

## 这个目录干啥

M3 的练习场。每个练习一个子目录，目录名即标题（动宾结构）。
M3 不是教学，是**让你犯错、然后修复错误的肌肉记忆**。

## 练习目录结构

```text
add-sysfs-to-gpio-rockchip/
├── README.md           为什么做这个、改前改后对比、实测结果
├── 0001-xxx.patch      你的修改（用 git format-patch -1 生成）
├── before.dmesg        修改前的 dmesg 截取
├── after.dmesg         修改后的 dmesg 截取
└── refs/               参考的 mainline commit / LWN 文章
```

## 练习类型与候选清单

| 类型 | 候选题目 | 难度 | 学什么 |
|------|---------|------|--------|
| 加 sysfs 节点 | 给 gpio-rockchip 加 `pin_state` 读节点 | ★ | DEVICE_ATTR、show/store |
| 加 module_param | 给 i2c-rk3x 加 `debug_mask` 开关 | ★ | module_param + dynamic debug |
| 加 dynamic debug | 在 spi-rockchip 加分层日志 | ★ | dyndbg=+pf 用法 |
| 适配新硬件 | 把 bmi160 挂到 RK3568 I2C2 上 | ★★ | dts overlay、device tree binding |
| 改时序/频率 | 把 i2c-rk3x 速率从 100k 改到 400k 看波形 | ★★ | 时钟、寄存器、示波器 |
| 改寄存器顺序 | 改 gpio-rockchip 初始化顺序观察影响 | ★★ | 硬件初始化时序 |
| BSP → mainline 靠拢 | 把 vendor 的 `kmalloc + kfree` 改成 `devm_kzalloc` | ★★★ | 错误路径、资源管理 |
| 把 platform_driver 改成 module 单例 | 反向小练习 | ★★ | 看清 platform 模型给你做了什么 |

## commit message 模板（按内核风格）

```text
<subsys>: <一句话动作>

<空一行>
<为什么改：现状什么问题、改了之后解决了什么>
<怎么改：核心思路 1-2 句>
<在 RK3568 上的实测结果：dmesg / 示波器 / 测速数字>

Signed-off-by: 你的名字 <你的邮箱>
```

**例子**：

```text
gpio: rockchip: add pin_state sysfs attribute for debugging

When porting a new GPIO-based sensor to RK3568, it's hard to know
the current direction/value without entering U-Boot. Add a per-pin
sysfs attribute under /sys/class/gpio/gpioN/ exposing direction and
level for read-only debug.

Tested on iTOP-RK3568 GPIO3_A4: 'cat pin_state' returns 'out=1'
matching the value pulled by a multimeter.

Signed-off-by: dengxy329-cmyk <...>
```

## 铁律提醒

- §8.9 **错误路径必须干净** —— 加了 sysfs 节点 probe 失败时记得 remove
- §8.8 **不要破坏用户态 ABI** —— sysfs 节点名一旦发布就不能改
- §8.6 先 work 再 right 再 fast —— 不要一上来追求优雅
- §5.2 改驱动 5 步法：找参考 → 复制改名 → 最小改动 → 跑通基线 → 增量迭代

## M3 自检（ROADMAP §11）

- [ ] 能在不破坏现有功能前提下加 sysfs 节点
- [ ] 错误回滚逻辑写得干净（probe 失败不漏资源）
- [ ] 每个修改有清晰的 commit message
