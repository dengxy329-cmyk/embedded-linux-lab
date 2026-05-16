# 50-subsystems / iommu

| 项 | 值 |
|----|---|
| 优先级 | P1 |
| AI 端侧相关度 | 高（AI 加速器都需要 IOMMU 做地址隔离） |
| 阶段 | M5 第 8 个 |
| 状态 | ⏳ 未开始 |

## 三件事

1. 框架：`iommu_ops` / `iommu_domain` / `iommu_group`
2. 读：`drivers/iommu/rockchip-iommu.c` + `drivers/iommu/arm/arm-smmu/`
3. 跑：看 RKNPU / ISP 用 IOMMU 时的页表设置

## 计划文件

- [ ] `00-framework.md`             —— IOMMU 总体模型
- [ ] `01-rockchip-iommu.md`
- [ ] `02-arm-smmu.md`              —— Qualcomm 路线用 SMMU
- [ ] `03-iommu-vs-swiotlb.md`      —— 没 IOMMU 时怎么办
- [ ] `04-interview-q.md`
- [ ] `refs/README.md`

## 关键问题

- 为什么 NPU/GPU 需要 IOMMU？（连续物理内存稀缺 + 安全隔离）
- `iommu_map` / `iommu_unmap` 在 driver 里怎么用
- DMA mapping API 怎么和 IOMMU 联动
- IOMMU group 与 PCIe 多功能设备的关系
