# {子系统名} / typical-impl

> 挑一个**有代表性**的驱动精读。能用 M2 的 case 笔记 ➜ 链接过去，不要重写。

## 选哪个

| 候选 | 理由 | 选定 |
|------|------|------|
| {例：drivers/gpio/gpio-rockchip.c} | RK3568 标准实现，~600 行刚好 | ✅ |
| {另一个候选} | ... | ❌ |

本次精读：**{选定的驱动}**

## 复用 M2 case 笔记

> {如果在 M2 已经读过} ➜ 直接链接：[case-NN-xxx](../../30-reading-drills/case-NN-xxx/)

下面只补充 M2 没写到的"子系统视角"分析。

## 这个驱动在子系统全景里的位置

```text
{子系统 framework}
    └─ {这个具体驱动}
        ├─ 注册成: xxx_chip
        ├─ 实现: xxx_ops 的哪些 callback
        └─ 不实现: 哪些（为什么不需要）
```

## 关键回调表

| 回调 | 在这个驱动里 | 调用时机 | 注意 |
|------|------------|---------|------|
| `xxx_request` | `rockchip_xxx_request` | userspace 请求资源时 | |
| `xxx_set` | `rockchip_xxx_set` | 写硬件 | spinlock 持有 |
| `xxx_get` | `rockchip_xxx_get` | 读硬件 | |
| `xxx_to_irq` | `rockchip_xxx_to_irq` | 关联中断 | |

## 寄存器映射

| 偏移 | 名称 | 用途 |
|------|------|------|
| 0x00 | XXX_CTRL | 控制 |
| 0x04 | XXX_STAT | 状态 |
| ... | ... | ... |

## 这个驱动的"个性"

{每个 vendor 的驱动都有它的"个性"——某些 quirk、某些遗留代码。}

- 个性 1：
- 个性 2：

## 学到的复用模式

- 模式 1：{例如：用 regmap 包裹 MMIO 屏蔽 endian}
- 模式 2：{例如：用 bitmap 跟踪已分配资源}
