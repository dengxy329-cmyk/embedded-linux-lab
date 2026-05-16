# 10-fundamentals / 内核速查（不是教程）

> 这不是教科书，是 M2-M5 撞墙时的速查手册。
> 读真实驱动遇到不懂的概念回这里翻表，**不复述 LDD3 / LKD 内容**。

## 这个目录干啥

把"驱动里最高频出现的 5 块知识"压成可查的一页表格。每条速查必须带：
- 一句话定义
- "什么时候用 / 什么时候用错"
- mainline 内核里的真实例子（commit hash + 文件链接）
- 在 RK3568 / QCS6490 上的特殊地址或约定

## 文件清单

| 文件 | 速查什么 | 关联 case |
|------|---------|----------|
| `memory-and-io.md` | ioremap、PHYS/VIRT、cache、DMA-coherent vs streaming | case-02 i2c-rk3x、case-04 spi |
| `concurrency.md` | spinlock / mutex / rwlock / RCU / seqlock 选哪个 | case-02 中断路径 |
| `interrupt-model.md` | top-half / threaded IRQ / softirq / tasklet / workqueue | case-02、case-04 |
| `kernel-objects.md` | kobject / device / driver / bus 关系图 | case-01 platform driver |
| `modern-patterns.md` | devm_* / IS_ERR / ERR_PTR / `__cleanup` / guard() | 所有 case |

## 何时写

- **不要 M2 之前一次性写完**——会写空
- **每读一个 case 撞到不懂的概念，就回这里补一条**
- 一条速查 ≤ 一屏内容（多了就拆）

## 速查模板

```markdown
## XXX

**一句话**：是什么。

**何时用**：场景。

**何时用错**：典型陷阱。

**RK3568 / QCS6490 上的具体值**：寄存器地址 / 中断号 / 时钟名。

**真实例子**：`drivers/xxx.c:123`（[mainline link](https://elixir.bootlin.com/...)）。

**面试题**：1-2 题 + 答案要点。
```

## 铁律提醒（ROADMAP §8.5）

- 只写"为什么"和"什么时候用错了"
- 不抄 LDD3 章节
- 每条都带一个 mainline 真实例子
