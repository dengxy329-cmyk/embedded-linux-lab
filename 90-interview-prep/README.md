# 90-interview-prep / 求职与晋升

> 短期目标：能胜任初级 Linux 驱动工程师。
> 中期：AI 端侧（Camera/ISP/NPU）专精。

## 这个目录干啥

按驱动面试高频考点分组，每篇至少 10 道题 + 答案要点。
**答案要直接背得过**，不是教程。

## 文件清单

| 文件 | 主题 | 题量目标 | 优先级 |
|------|------|---------|--------|
| `q-platform-driver.md` | 平台驱动、设备树、probe 流程 | 15 | P0 |
| `q-concurrency.md` | spinlock / mutex / RCU / 中断上下文 | 15 | P0 |
| `q-memory-and-dma.md` | ioremap / DMA mapping / dma-buf / IOMMU | 15 | P0 |
| `q-debug-scenarios.md` | 给场景问怎么排（ftrace / kgdb / oops） | 10 | P1 |
| `q-ai-camera-specific.md` | V4L2 / MIPI CSI / ISP / Camera pipeline / NPU | 15 | P1（AI 方向必备） |
| `system-design.md` | "设计一个 XX 子系统驱动" 类大题 | 5 | P2 |

## 每题模板

```markdown
### Q: <问题>

**考点**：<一句话>

**答案要点（背诵版）**：
- 点 1
- 点 2
- 点 3

**追问**：
- 如果用 spinlock 替换会怎样？
- 中断里能 sleep 吗？为什么？
- ...

**真实代码例子**：`drivers/xxx.c:123` ([elixir link](https://elixir.bootlin.com/...))

**陷阱**：面试官最爱挖的坑。
```

## 例题（放 3 个示范，模板成色直接抄）

### Q1: probe 函数返回 -EPROBE_DEFER 内核会怎么处理？

**考点**：driver core、依赖管理。

**答案要点（背诵版）**：
- 这个返回值告诉内核"我依赖的某个资源（clock/regulator/iommu）还没准备好"
- driver core 把这个 device 放进 deferred probe list
- 任何新 driver/device 注册成功时，触发 deferred list 重试
- 启动末期还有未完成的 deferred probe，dmesg 报警告

**追问**：
- 为什么不直接死循环重试？→ 浪费 CPU、可能永远不会成功
- 哪些 API 会自动返回这个？→ `devm_clk_get`、`devm_regulator_get`、`of_iommu_get` 等
- 怎么调试 deferred probe 死锁？→ `cat /sys/kernel/debug/devices_deferred`

**真实代码例子**：`drivers/base/dd.c:driver_deferred_probe_trigger()`

**陷阱**：probe 里如果在拿到资源之前申请了内存/中断没释放，return -EPROBE_DEFER 时会泄漏 → 这就是为什么要 `devm_*`。

---

### Q2: spinlock 和 mutex 怎么选？为什么不全用 mutex？

**考点**：并发原语、上下文。

**答案要点（背诵版）**：
- **能不能 sleep** 是分界：spinlock 不能 sleep（持有时禁抢占/禁中断），mutex 可以 sleep
- **中断上下文必须 spinlock**：mutex 会 sleep → 中断里 sleep = panic
- **进程上下文长临界区** 用 mutex（让出 CPU 给别人）
- **极短临界区**（< 100 行汇编可完成）用 spinlock，不值得切换上下文

**追问**：
- 中断里 spin_lock 还是 spin_lock_irqsave？→ 如果同一锁也在线程上下文用，必须 irqsave
- 读多写少呢？→ rwlock 或 RCU
- mutex 内部怎么实现？→ 快路径是 atomic + futex，慢路径才进调度器

**真实代码例子**：`drivers/i2c/busses/i2c-rk3x.c` 中断处理用 `spin_lock`，transfer 函数用 `mutex`

**陷阱**：在 spinlock 持有期间调用任何可能 sleep 的函数（`copy_from_user`、`kmalloc(GFP_KERNEL)`、`clk_prepare`）→ 内核会喊 "BUG: sleeping function called from invalid context"。

---

### Q3: devm_kzalloc 失败时要不要释放？probe 失败时要不要手动 free？

**考点**：错误路径、资源管理（铁律 §8.9）。

**答案要点（背诵版）**：
- `devm_kzalloc` 失败返回 NULL，**不需要手动 free**（没分配出来就没东西释放）
- probe 函数 **return 任何错误值**时，driver core 会自动调所有 `devm_*` 注册过的清理函数
- 这就是为什么强制 `devm_*` —— 你不写 `goto err_xxx` 链也安全
- 但**注意顺序**：注册顺序倒着释放（栈式），所以注册顺序就是依赖顺序

**追问**：
- `devm_kmalloc` 什么时候被释放？→ device detach 时（probe 失败、driver unbind、模块卸载）
- 能在 remove 函数里再次 free 吗？→ 不能，会 double free
- 如果 probe 成功后某个时候才分配呢？→ 用普通 kzalloc + 自己管，或 `devres_add` 手动加进 devres 链表

**真实代码例子**：所有 `drivers/gpio/gpio-rockchip.c` probe 路径

**陷阱**：混用 `devm_*` 和裸 API（一半 devm，一半手动 free）→ remove 时双重释放。原则：**全 devm 或全手动，不混用**。

## 收集来源

- 牛客 / LeetCode 讨论区"Linux 驱动面试"标签
- 你实际面到的题（**最有价值，必须记**）
- 你师兄 / 实习同事的真题
- 公司内部技术分享提到的难题

## 风格（铁律 §8.5）

- 答案要简短可背诵（一屏内）
- 必须带"为什么"，不只是"是什么"
- 每题挂一个真实内核代码引用
- 不写 OSTEP/APUE 章节式的长篇

## AI Camera 方向特别提醒

`q-ai-camera-specific.md` 是你的差异化优势：
- V4L2 subdev 模型
- MIPI CSI 时序
- ISP pipeline (rkisp1 / Spectra)
- DMA-BUF / dma-heap 在 Camera-NPU 间共享 buffer
- IOMMU 给 NPU 做隔离

这些一般面试官只问框架，能答出来一两个具体的内核结构体（如 `struct v4l2_subdev`）就能区分出来。
