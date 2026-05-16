# 99-resources / 资源、书签、速查

> 把 ROADMAP §10 的资源清单分门别类落地，加上自己用过/没用过的标记。

## 这个目录干啥

外部资源的"门牌号"。读完任何一篇 LWN / 视频 / 邮件 thread，回这里登记一行"它解决了我什么问题"。

## 文件清单

| 文件 | 内容 |
|------|------|
| `books-and-articles.md` | 书、LWN 收藏、必读 paper、博客 |
| `mailing-lists.md` | 邮件列表订阅状态 + 关注的 thread |
| `youtube-conference-talks.md` | LPC / ELC 必看视频 + 笔记 |
| `cheatsheets/` | 一页速查（命令/API 对照表） |

## cheatsheets/ 清单

| 文件 | 内容 |
|------|------|
| `git-kernel.md` | format-patch / send-email / get_maintainer / checkpatch |
| `devm-functions.md` | 全部 devm_* 函数对照表 + 何时用 |
| `of-property-read.md` | of_property_read_* 系列对照（u32/string/array...） |
| `ftrace.md` | tracefs 常用命令（events、function_graph、trace_pipe） |

## 收藏规则（铁律 §8.5）

- 任何 LWN 链接收藏前必须**读完**
- 不光收藏，必须写 1 句"这个解决了我什么问题"
- 视频笔记必须带时间戳（如 `12:30 - 讲到 IOMMU 配置`）

## 收藏分级

| 等级 | 何时给 | 例子 |
|------|--------|------|
| ⭐⭐⭐ | 改变工作方式 | LWN "Notes from a container" 系列 |
| ⭐⭐ | 解了一个具体问题 | StackOverflow / Rockchip 论坛某帖 |
| ⭐ | 知识扩展 | 一般 LPC talk |
| 不收藏 | 看完忘了 | - |

## 必装工具速查（ROADMAP §10）

```text
clangd            代码索引、跳转
ctags / cscope    旧风格但快
ripgrep (rg)      比 grep 快 10 倍
fzf               模糊查找
tig               git 可视化
htop / btop       系统状态
minicom/picocom   串口
bpftrace          内核追踪
trace-cmd         ftrace 前端
perf              性能采样
bear              生成 compile_commands.json
dtc               设备树编译
device-tree-compiler  同上
```

## 必订阅（每周看）

- [LWN.net](https://lwn.net/) 周刊（$9/月可看新文章，免费晚一周）
- [Bootlin Embedded Linux Training](https://bootlin.com/training/) 材料免费下载
- 你的子系统 mailing list（见 `mailing-lists.md`）

## 必看视频（看一遍胜过 LDD3 一章）

- Linux Plumbers Conference 历年
- Embedded Linux Conference 历年
- [Bootlin YouTube](https://www.youtube.com/@FreeElectrons)
- KernelRecipes 系列
