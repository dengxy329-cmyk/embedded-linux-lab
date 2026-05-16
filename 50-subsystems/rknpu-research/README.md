# 50-subsystems / rknpu-research（闭源逆向）

| 项 | 值 |
|----|---|
| 优先级 | P1 |
| AI 端侧相关度 | **高**（RK3568 上的 AI 加速器） |
| 阶段 | M5 第 10 个 |
| 状态 | ⏳ 未开始 |

## 这个目录干啥

RKNPU 驱动**部分闭源**，driver 不在 mainline，BSP 里也只能看到 ioctl 接口 + 部分实现。
方法（ROADMAP §4 M5）：**看用户态 SDK 的 ioctl 调用反推 driver 接口**。

## 计划文件

- [ ] `00-rknpu-overview.md`        —— RKNPU 硬件 + 软件栈
- [ ] `01-userland-sdk.md`          —— RKNN SDK 用法 + ioctl trace
- [ ] `02-ioctl-decode.md`          —— 抓到的 ioctl 序号 + 参数结构猜测
- [ ] `03-driver-skeleton.md`       —— 推测的 driver 接口
- [ ] `04-rkservr-and-init.md`      —— 用户态 daemon 启动流程
- [ ] `05-interview-q.md`
- [ ] `refs/README.md`

## 方法工具

```text
strace -f -e ioctl <rknn 程序>        抓 ioctl 序号
ltrace                                 抓用户态库调用
ftrace events vfs/ioctl/sys_enter     内核侧
bpftrace 'tracepoint:syscalls:sys_enter_ioctl /comm == "rknn_xxx"/ { ... }'
```

## 关键 ioctl 类型（先验）

每个 AI 加速器驱动都会有这些类型，先猜出来再验证：
- `ALLOC` —— 分配 dma-buf
- `MAP` —— 映射到设备地址空间
- `SUBMIT` —— 提交一个 inference job
- `WAIT` —— 等 job 完成
- `FREE` —— 释放 buffer
- `QUERY_INFO` —— 查询硬件信息

## 输出（M5 结束时）

一份"RKNPU driver 接口推测图" + 一段能用 ioctl 直接调用的 C 代码。
