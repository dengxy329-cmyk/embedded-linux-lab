# {子系统名} / my-toy

> 自己写的简化版"玩具驱动"。可选——复杂子系统（V4L2、DRM）不强求。
> 目标：把"读懂别人的驱动"升级为"自己造一个最小可用的"。

## 这个玩具长什么样

| 项 | 值 |
|----|---|
| 名字 | fake-{子系统}-toy |
| 硬件 | 模拟（无硬件）/ RK3568 上某外设 |
| 大小目标 | ≤ 300 行 |
| 功能 | 暴露一个 sysfs，注册到 {子系统}，userspace 能用 |

## 文件结构

```text
my-toy/
├── Makefile
├── fake-xxx.c
├── Kconfig（可选）
├── README.md
└── test/
    └── user.c   userspace 测试程序
```

## 跑通的步骤

```bash
# 在 VM 里编
make -C /path/to/kernel M=$(pwd) modules

# 拷到 NFS root，板子上
insmod fake-xxx.ko
ls /sys/class/{子系统}/
echo 1 > /sys/class/{子系统}/xxxN/some_attr
dmesg | tail
```

## 关键代码片段

### 注册到子系统

```c
{贴 10-15 行核心注册代码，不要全文}
```

### 一个 callback 实现

```c
{贴一个有代表性的 ops 实现}
```

## 跟"读过的真实驱动"的差异

| 维度 | 真实驱动 | 我的玩具 |
|------|---------|---------|
| 硬件交互 | writel / readl | 模拟变量 |
| 并发保护 | spinlock + IRQ | 单线程，简化 |
| 错误处理 | 完整 devm_ + goto | 只走 happy path |
| sysfs 节点 | 5+ | 1 |

## 学到了什么

- 没读过 ≠ 不会写；写过 ≠ 真懂
- 自己写时才知道注册流程的"魔法"在哪
- {你自己的发现}

## 1-2 道面试题

### Q: 写一个最小的 {子系统} driver 至少需要哪几步？

{答案要点}
