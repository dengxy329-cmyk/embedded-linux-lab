# 50-subsystems / dma-buf ⭐⭐⭐

| 项 | 值 |
|----|---|
| 优先级 | **P0**（端侧 AI 内存核心） |
| AI 端侧相关度 | **高**（Camera ↔ NPU ↔ Display 零拷贝） |
| 阶段 | M5 第 7 个（**重锤砸**） |
| 状态 | ⏳ 未开始 |

## 三件事

1. 框架：`dma_buf` / exporter / importer / sync / fence
2. 读：`drivers/dma-buf/dma-buf.c` + `drivers/dma-buf/heaps/`
3. 写一个 demo：V4L2 capture 出的 buffer → 模拟 NPU 处理 → 显示，全程零拷贝

## 计划文件

- [ ] `00-framework.md`             —— dma-buf 总览（exporter / importer / sync）
- [ ] `01-dma-heaps.md`             —— dma-heap（取代 ION）
- [ ] `02-fence-and-sync.md`        —— fence / sync_file 跨设备同步
- [ ] `03-v4l2-to-npu-demo.md`      —— 零拷贝 pipeline 实战
- [ ] `04-cache-coherency.md`       —— ARM SMMU + cache 维护
- [ ] `05-interview-q.md`
- [ ] `refs/README.md`

## 关键问题

- dma-buf 怎么跨进程？fd 传递机制
- exporter 提供哪些 ops？map_dma_buf / unmap_dma_buf
- 为什么需要 fence？异步操作的顺序约束
- 与 IOMMU 的配合（见隔壁 iommu 子系统笔记）

## 必读资源

- LWN: "DMA-BUF cache handling" 系列
- LPC: "DMA-BUF heaps" 历年 talk
- mainline Documentation: `Documentation/driver-api/dma-buf.rst`

## 与 NPU 的关系

RKNPU / Hexagon 都通过 dma-buf 拿到 Camera 输出 buffer。
理解这个 = 理解整个端侧 AI 数据流。
