# AGENTS.md

> 给接续协助的 AI 模型（Cursor / Claude / Codex / 其他）的快速指令。
> **你必须先读 [ROADMAP.md](./ROADMAP.md)（学习宪章），然后再读本文件。**

## 学习者上下文（30 秒版）

| 项 | 值 |
|----|---|
| 身份 | 研一，端侧 AI 公司实习生 |
| 已完成 | OSTEP / APUE / 教科书 cdev 驱动（M0） |
| 当前 | "教科书 → 工程能用" 进阶路线（M1-M7） |
| 板子 | RK3568（讯为 iTOP） + Radxa Dragon Q6A（QCS6490） |
| 未来项目 | QCS8300 车载 AI 相机 |
| 当前阶段 | 见 [ROADMAP.md §11 进度看板](./ROADMAP.md#11-进度跟踪与自检) |

## 协助原则（不可违反）

1. ❌ 不要按 OSTEP / APUE 章节讲（M0 内容，已完成）
2. ❌ 不要从 `hello world` cdev 开始
3. ❌ 不要堆砌"理论 + 教科书例子"
4. ✅ 给"在 RK3568 / QCS6490 上怎么做"的具体步骤
5. ✅ 笔记按 [ROADMAP §6 仓库结构](./ROADMAP.md#6-仓库结构) + [§5.4 案例驱动法](./ROADMAP.md#54-笔记-案例驱动-法)
6. ✅ 严格遵守 [§8 10 条铁律](./ROADMAP.md#8-核心原则10-条铁律)
7. ✅ 区分 [§9 Rockchip vs Qualcomm 差异](./ROADMAP.md#9-rockchip-vs-qualcomm-工程差异重要)

## 输出风格（不可违反，参考 STYLE.md）

- 直白精简，**表格优于段落**
- 必须带面试题及答案（每篇至少 1-2 道）
- 可以直接 commit 进仓库（按 [STYLE.md](./STYLE.md)）
- 中文为主，技术名词保留英文
- 不写注释解释代码做了什么，只写**为什么**（铁律 §8.5）

## 笔记落点速查

| 内容类型 | 落点 |
|---------|------|
| M1 环境搭建 | `00-env/` |
| 内核速查 | `10-fundamentals/` |
| 驱动模型 | `20-driver-model/` |
| 读代码笔记（M2） | `30-reading-drills/case-NN-xxx/` |
| 改代码记录（M3） | `40-modification-drills/<动作>/` |
| 子系统点亮（M5） | `50-subsystems/<子系统>/` |
| 调试工具 / war story（M4） | `60-debug-arsenal/` |
| 构建系统（M6） | `70-build-systems/` |
| 对外贡献 patch（M6） | `80-patches/NNN-<标题>/` |
| 面试题 | `90-interview-prep/` |
| 资源 / 速查 | `99-resources/` |

## 文件操作时的硬约束

- 新建笔记前先看对应子目录有没有 README.md，按 README 里的"计划文件"清单填，不要自创结构
- 案例笔记必须 cp `30-reading-drills/templates/` 里的模板，不要重新发明
- 子系统笔记必须 cp `50-subsystems/templates/` 里的模板
- commit message 按 [STYLE.md §7](./STYLE.md) 内核风格（英文 subject，body 解释 why）

## 不确定时

| 不确定什么 | 查哪里 |
|------------|--------|
| 文档结构 | [ROADMAP.md §6](./ROADMAP.md#6-仓库结构) |
| 方法论 | [ROADMAP.md §5](./ROADMAP.md#5-学习方法论) |
| 笔记风格 | [STYLE.md](./STYLE.md) |
| 调试方法 | [ROADMAP.md §5.3](./ROADMAP.md#53-调试-5-步法) + `60-debug-arsenal/` |
| 还是不确定 | 在回答里明确说"我不确定 X，建议你 Y"，**不要瞎猜** |

## 反例（看到自己在做这些 → 立刻停下）

- "我来给你讲一下 OSTEP 第 X 章对应的内核实现……"
- "首先我们写一个简单的字符设备驱动……"
- "根据 APUE，fork 的语义是……"
- "可以参考 LDD3 第 X 章的例子……"
- 给一段教科书例子代码，没有 RK3568/QCS6490 上下文
- 把 cdev / file_operations / chrdev_alloc 当作"基础"再讲一遍

## 修订历史

| 版本 | 日期 | 修订 |
|------|------|------|
| v1.0 | 2026-05-16 | 初版，配合 ROADMAP v1.0 + STYLE v1.0 |
