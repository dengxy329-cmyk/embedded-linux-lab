# 60-debug-arsenal / 调试武器库（M4）

> **每个工具至少破解过一个真 bug** —— 这是 M4 的交付物。
> 工具不会用 ≠ 没听说过，差就差在"撞过 N 个真 bug 之后的肌肉记忆"。

## 这个目录干啥

把 8 个调试工具从"听说过"变成"用过"。每个工具一篇速查 + 至少一篇 war story。
跟 M2/M3/M5 穿插进行——读代码遇到问题就动手用，不要等"M4 周"集中学。

## 工具清单

| 文件 | 工具 | 用在什么场景 | 何时学 | 状态 |
|------|------|------------|--------|------|
| `printk-and-dynamic-debug.md` | printk / pr_debug / dyndbg | 日志追踪、运行时开关 | M2 第一周 | ⏳ |
| `ftrace.md` | ftrace events + function_graph | 函数级时序、子系统事件 | M2 case-02 中 | ⏳ |
| `bpftrace-oneliners.md` | bpftrace | 一行命令快速抓 | M3 改驱动验证时 | ⏳ |
| `perf.md` | perf top / trace / record / report | 性能采样、热点定位 | M5 v4l2 性能优化时 | ⏳ |
| `kgdb-on-rk3568.md` | kgdb（串口） | 板上下断点、单步 | M3 中后段 | ⏳ |
| `oops-panic-cookbook.md` | oops 手工分析 | 崩溃排查（无 kgdb 时） | 一旦碰到第一个 oops | ⏳ |
| `crash-tool.md` | crash | dump 分析、看现场寄存器/栈 | M5 后期 | ⏳ |

## war-stories/ 是这个目录的灵魂

每篇 war story 格式（**重点：写"假设链"，不要只写结论**）：

```markdown
# 001-i2c-rk3x 死锁问题

## 现象
- dmesg 报 `rcu_sched stalls`
- /sys/class/i2c-dev/i2c-2/device/runtime_status 卡在 suspending

## 用了什么工具
- ftrace function_graph 跟踪 i2c_xfer
- bpftrace 抓 spin_lock_irqsave 持有时间

## 假设链（重点！）
1. 假设 A：硬件挂了 → 用示波器看 SCL/SDA → 否决，时钟在跑
2. 假设 B：中断丢了 → ftrace 看 i2c_isr → 中断在进
3. 假设 C：spin_lock 自旋等不释放 → bpftrace 一行抓到了 → 命中

## 根因
（一行）`i2c_rk3x_set_clk_div()` 在中断上下文里调了可能 sleep 的 clk_prepare。

## 修复
- commit hash / patch 链接
- 改成 atomic 版本

## 教训
- 中断上下文里禁止 sleep（已知，但实战中容易混入间接调用）
- 下次先 grep 中断处理路径里有没有 `clk_prepare` / `pinctrl_select` 这类潜在 sleep
```

## 命名规则

```text
war-stories/
├── NNN-<subsys>-<现象>.md
├── 001-i2c-rk3x-rcu-stall.md
├── 002-gpio-rockchip-probe-defer-loop.md
└── ...
```

## M4 自检（ROADMAP §11）

- [ ] ftrace 抓过一个真实驱动的执行流
- [ ] bpftrace 写过至少 3 个一行命令
- [ ] 解读过一次完整的 oops 现场
- [ ] kgdb 在 RK3568 上设过断点

## 工程师生存技能（铁律 §8.10）

不要怕板子崩。内核 panic 是日常：
- 每次大改前 SD 卡镜像 snapshot
- 板子崩了 5 分钟内重烧固件
- 把 panic 当成"工具给你的免费 dump"，而不是失败
