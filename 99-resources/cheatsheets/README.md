# cheatsheets / 一页速查

> 每张速查 ≤ 一屏。多了拆。

## 文件清单

| 文件 | 速查什么 | 何时写 | 状态 |
|------|---------|--------|------|
| `git-kernel.md` | format-patch / send-email / get_maintainer / checkpatch | M1 D10 写一版 | ⏳ |
| `devm-functions.md` | 全部 devm_* 函数对照表 + 何时用 | M2 case-01 时写 | ⏳ |
| `of-property-read.md` | of_property_read_* 系列对照（u32/string/array...） | M2 case-01 时写 | ⏳ |
| `ftrace.md` | tracefs 常用命令（events / function_graph / trace_pipe） | M4 写 | ⏳ |

## 写作原则

- **不抄 manual**，抄了等于没写
- **每条带"我什么时候用过它"**（一句话上下文）
- **错误用法也列**（最容易忘的部分）
- 表格优于段落

## 模板

```markdown
| 命令 / API | 干啥 | 何时用 | 何时用错 | 例子 |
|-----------|------|--------|---------|------|
| `git send-email --to=foo --cc=bar a.patch` | 发 patch | 提主线时 | 没配 SMTP 会卡半天 | M6 第一个 patch |
```

## 候选可加的速查（先记着）

- `printk-loglevels.md`        —— KERN_INFO/WARNING/ERR 区别
- `dt-bindings-yaml.md`        —— YAML schema 关键字段
- `kbuild-syntax.md`           —— Makefile.build 关键变量
- `kgdb-quickref.md`           —— kgdb 命令对照
- `bpftrace-recipes.md`        —— 常用 bpftrace 一行
