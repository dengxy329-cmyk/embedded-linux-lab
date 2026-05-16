# {Case 标题} / modifications（M3 阶段填）

> 这里记录在这个驱动上做过的"外科手术"。每个修改一段，必须能独立 commit。

## 修改 1：{动作标题}

| 项 | 值 |
|----|---|
| 日期 | YYYY-MM-DD |
| commit hash | abc1234 |
| 目的 | {一句话} |
| 范围 | {改了几行 / 几个文件} |

### 为什么改

{现状什么问题、改了之后解决了什么。}

### 怎么改

{核心思路 1-2 句。代码 diff 不要全贴，贴关键 5-15 行即可，其余给 commit hash。}

```diff
- old line
+ new line
```

### 在 RK3568 / QCS6490 上的实测结果

| 测试项 | 修改前 | 修改后 |
|--------|--------|--------|
| dmesg 输出 | xxx | yyy |
| sysfs 节点 | 没有 | `/sys/.../pin_state` 可读 |
| 性能 | xxx | yyy |

### 错误路径验证（铁律 §8.9）

- [ ] probe 半路失败时资源都释放了
- [ ] rmmod 没 oops
- [ ] 在 dmesg / kmemleak 里没看到泄漏

### 学到了什么

- {例如：第一次明白 `device_create_file` 的失败要怎么 rollback}

---

## 修改 2：{下一个动作标题}

（同上结构）

---

## 没做但想做的（backlog）

- [ ] {例如：把这个驱动的 ID-table 从 of_device_id 改成 acpi 兼容}
- [ ] {例如：尝试用 cleanup attribute (__free) 简化错误路径}
