# 50-subsystems / drm-display

| 项 | 值 |
|----|---|
| 优先级 | P2 |
| AI 端侧相关度 | 低（Camera 链路通常不直接接 DRM） |
| 阶段 | M5 第 9 个（按需） |
| 状态 | ⏳ 未开始 |

## 三件事

1. 框架：DRM / KMS / atomic modeset
2. 读：`drivers/gpu/drm/rockchip/`（VOP2）
3. 改：可选，跑通一个屏

## 计划文件

- [ ] `00-framework.md`
- [ ] `01-rockchip-drm.md`
- [ ] `02-atomic-modeset.md`
- [ ] `03-interview-q.md`
- [ ] `refs/README.md`

## 关键问题

- KMS 三件套：crtc / encoder / connector
- atomic modeset 状态机
- DRM 和 V4L2 通过 dma-buf 配合（如果项目需要拍照 → 实时显示）

## 为什么 P2

- AI Camera 项目里显示一般不是核心痛点
- 如果你的端侧 AI 项目需要在屏上预览，这里至少要做完 framework 和 atomic
