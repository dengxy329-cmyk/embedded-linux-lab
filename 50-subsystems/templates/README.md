# 50-subsystems / templates

> M5 开子系统时 `cp -r 50-subsystems/templates 50-subsystems/<新子系统>` 直接用。

## 5 件套

| 文件 | 内容 |
|------|------|
| `00-framework.md` | 子系统总览：核心结构体、注册流程、调用关系 |
| `01-typical-impl.md` | 挑一个典型驱动精读（可复用 M2 case 笔记） |
| `02-mainline-vs-bsp.md` | vendor 改了什么、为什么 |
| `03-my-toy.md` | 自己写的简化版（可选，复杂子系统不强求） |
| `04-interview-q.md` | 面试高频题 + 答案要点 |

加 `refs/` 子目录存外部参考。

## 与 M2 case 的关系

- M2 阶段读到的 case 可以直接成为 M5 该子系统的 `01-typical-impl.md`
- 不要重复写，**链接过去**即可
