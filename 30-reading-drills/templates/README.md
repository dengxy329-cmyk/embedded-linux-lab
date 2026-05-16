# 30-reading-drills / templates

> M2 开 case 时 `cp -r 30-reading-drills/templates 30-reading-drills/case-XX-yyy` 直接用。

## 6 件套

| 文件 | 内容 | 何时填 |
|------|------|--------|
| `00-overview.md` | case 元信息、为什么挑、硬件背景、学习目标 | 开 case 第 1 天 |
| `01-dts.md` | dts 节点 + bindings YAML | 开 case 第 1 天 |
| `02-walkthrough.md` | probe 逐段解析 + mermaid 调用图 | 开 case 第 2-3 天 |
| `03-trace.md` | 一次完整 read/write/ioctl 从用户态追到寄存器 | 开 case 第 3-4 天 |
| `04-modifications.md` | M3 阶段的修改记录 | M3 |
| `05-debug-log.md` | 踩坑/调试记录 | 持续 |

## 别忘了

- 每个 case 一个 `refs/README.md`，存所有参考过的 commit/LWN/邮件
- 严格按 [STYLE.md](../../STYLE.md) 的链接格式 + commit message 规范
- 不要堆教科书内容（[AGENTS.md](../../AGENTS.md) 反例清单看一遍）
