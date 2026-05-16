# STYLE.md / 笔记风格手册

> 任何笔记一打开就应该长什么样。
> 跟 [ROADMAP.md](./ROADMAP.md) §5.4 案例驱动法 + §8 10 条铁律 配合使用。

## 1. 语言

| 内容 | 用什么 |
|------|--------|
| 笔记正文 | 中文 |
| 技术名词 | 保留英文（`platform_driver`、`devm_kzalloc`、`IRQ_HANDLED`） |
| 代码块（含注释） | 英文 |
| commit message | **英文**（按内核 patch 风格，未来提主线时手不变形） |
| 面试题答案 | 中英混合（要点中文、API/结构体英文） |
| 文件名 | 全小写英文 + 短横线 |

## 2. 标点

- 中文段落用中文标点："，。；："
- 英文专有名词后跟中文，无需空格："使用 `devm_kzalloc` 分配内存"
- 数字 + 单位：50 行（数字和单位之间空一格中文）；`50ms`（代码里不空格）
- 不滥用中文括号"（）"——文件名/路径里用英文 `()`

## 3. 表格优于段落

3 项以上的并列内容**必须**用表格。
段落只用于：
- 单一陈述
- 推理链 / 假设链
- 故事性叙述（war story、踩坑回忆）

## 4. 链接格式

| 类型 | 写法 |
|------|------|
| mainline 源码 | `<https://elixir.bootlin.com/linux/v6.6/source/drivers/.../xxx.c>` |
| mainline 带行号 | `xxx.c:123` + 完整 elixir 链接 |
| BSP 源码 | `<https://github.com/rockchip-linux/kernel/blob/develop-6.1/drivers/.../xxx.c>` |
| commit hash | `[abc1234](URL)` |
| LWN 文章 | `[标题](URL)`，并写一句"它解决了我什么问题" |
| 内部链接 | `[文件名](./相对路径)` |
| 邮件 thread | `[Subject](lore.kernel.org/...)` |

## 5. 图表

| 类型 | 工具 | 何时用 |
|------|------|--------|
| 流程图 / 状态机 / 类关系 | **mermaid**（GitHub 自动渲染） | 优先 |
| 文件树 / 简单调用栈 | ASCII art（用 ` ```text ` 块） | 简单时 |
| 时序图 | mermaid `sequenceDiagram` | 多方交互 |
| 示波器 / 截图 | png 放到 `case-XX/refs/` | 必要时 |
| 复杂硬件框图 | 找官方资料 + 链接 + 自己标注 | 必要时 |

## 6. 文件命名

| 规则 | 例子 |
|------|------|
| 全小写 + 短横线 | `nfs-root-loop.md`，不是 `NFSRootLoop.md` |
| Case 加编号 | `case-01-gpio-rockchip/` |
| war story 加编号 | `001-i2c-rk3x-rcu-stall.md` |
| patch 加编号 | `001-dt-bindings-typo/` |
| 面试题文件用 `q-` 前缀 | `q-platform-driver.md` |
| 模板文件用原始名 | 模板就叫 `00-overview.md.template`？**不**，模板就放 `templates/` 目录下叫 `00-overview.md` |

## 7. commit message（铁律 §8.3）

按内核风格，三段：

```text
<subsys>: <一句话动作>（subject，< 75 字符，英文）

<空一行>
<为什么改：现状什么问题、改了之后解决了什么>
<怎么改：核心思路 1-2 句>
<在 RK3568 上的实测结果>（可选）

Signed-off-by: 你的名字 <你的邮箱>
```

`<subsys>` 例子：
- 笔记：`docs`、`scaffold`、`m1`、`case-01`、`subsys-gpio`
- 代码：`gpio`、`i2c-rk3x`、`drivers/iio/bmi160`

笔记 commit 也按这个格式，养成习惯。

**反例**：
- ❌ `update README` —— 没说改了什么、为什么
- ❌ `add stuff` —— 灾难
- ❌ `修了一个 bug` —— 没说什么 bug、怎么修

**正例**：

```text
case-01: capture probe path walkthrough with mermaid graph

The first reading drill on gpio-rockchip needs a single-page
overview of what happens between platform_driver_register() and
the gpio_chip being usable. Add a mermaid call graph plus a side
table mapping each callback to the resource it allocates.

Signed-off-by: dengxy329-cmyk <...>
```

## 8. emoji

| 用 | 不用 |
|----|------|
| ⭐ 表示优先级 / 重点 | 装饰用 emoji |
| ⏳ 🔵 ✅ ❌ 表示状态 | 表情包 |
| - | 段落里的 emoji（正文别加） |

## 9. 行尾 / 编码

- 行尾：**LF**（不要 CRLF）—— 看 `.gitattributes`
- 编码：**UTF-8 without BOM**
- 缩进：markdown 列表用 2 空格，代码块按语言习惯
- 文件末尾：保留一个空行
- Tab vs Space：内核 C 代码用 **Tab**（hard tab，宽度 8），笔记 markdown 用 **2 空格**

## 10. 长度上限

| 文件类型 | 建议长度 |
|---------|---------|
| README.md（导览） | 50-150 行 |
| 速查文件（cheatsheet） | ≤ 1 屏（~50 行） |
| 案例笔记（walkthrough） | 100-300 行 |
| war story | 50-150 行 |
| 面试题文件 | 每题 30-50 行 × 10-15 题 |

**超过上限 → 拆。** 不要写"超长 README"——读者会跳过。

## 11. 内部 / 外部材料隔离

- 公司脱敏前的草稿放 `internal/`（已在 .gitignore 永不 commit）
- 公开版放对应 case 目录（脱敏后）
- 大文件（dts 全文、kernel config）**不放仓库**，笔记里贴链接 + 关键片段
- 截图：能用文字描述的不放图，必要的图压缩 < 200KB

## 12. 引用 mainline 代码的"诚信规则"

- 引一段超过 5 行的代码 → 必须给出 elixir 链接（让读者验证）
- 引 commit hash → 必须能 fetch 到（v6.6 mainline 或 vendor BSP）
- 引"我看了 XX" → 不能写"应该是这样"，要么验证，要么标"未验证假设"

## 修订历史

| 版本 | 日期 | 修订 |
|------|------|------|
| v1.0 | 2026-05-16 | 初版 |
