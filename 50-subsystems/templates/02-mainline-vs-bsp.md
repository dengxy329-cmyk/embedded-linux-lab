# {子系统名} / mainline-vs-bsp

> Vendor 在 mainline 基础上改了什么、为什么改。这是 M2/M5 最有价值的工程视角。

## 选哪个文件对比

- mainline: `drivers/.../xxx-rockchip.c` (v6.6)
- BSP: `drivers/.../xxx-rockchip.c` (rockchip-linux/kernel develop-6.1)

## 怎么对比

```bash
# 在 BSP 仓库
git log --oneline drivers/.../xxx-rockchip.c | head -30

# 拿到 BSP 最近改动后，对应到 mainline 对应版本
diff <(curl mainline-raw) <(cat bsp-file)
```

## diff 摘要

| 类型 | 数量 | 总览 |
|------|------|------|
| BSP 新增的函数 | N 个 | 主要是 quirk |
| BSP 删除的代码 | N 行 | （通常很少） |
| 算法/状态机改动 | M 处 | 重点看 |
| Debug 增强 | K 处 | 学习 vendor 怎么写日志 |

## 每个差异的"为什么"

### 差异 1：BSP 新增了 `rockchip_xxx_quirk`

**BSP 代码**：

```c
if (chip->revision == 0x10)
    val |= RK3568_XXX_QUIRK;
```

**mainline 没有**：

**vendor 改的原因**（推测/查证）：
- {例如：某硬件批次有 bug，rev 0x10 需要特殊 bit}
- {如果能找到 BSP commit message，贴一下}

**应该 upstream 吗**：
- 是 / 否，理由

### 差异 2：BSP 删了 `xxx_thermal_warn`

**BSP 删了什么**：

**mainline 还在**：

**vendor 删的原因**：
- {例如：测温节点上游版本不支持，被 vendor 屏蔽}

---

## 总体判断

| 维度 | 评价 |
|------|------|
| BSP 改的合理度 | 高 / 中 / 低 |
| 多少 vendor patch 应该 upstream | N% |
| 有没有违反 §8 铁律的写法 | 例：裸 kmalloc 还在 |

## 给自己的提示

- 如果未来要把 vendor 改动同步到 mainline，先做什么
- 如果未来读公司代码，类似 quirk 怎么定位
