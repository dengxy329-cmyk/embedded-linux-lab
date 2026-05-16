# 80-patches / 对外贡献（M6）

> 提一个主线 patch（哪怕只是修个 typo / 改个 dt-bindings 文档），把流程跑通。
> 不强求 patch 被合，**走完一遍流程 + 收到 maintainer review** 就算赢。

## 这个目录干啥

每个 patch 一个子目录，存放：
- `0001-xxx.patch` —— 真正发出去的 patch
- `cover-letter.txt` —— 多 patch 时的封面信
- `lkml-thread.md` —— maintainer 回复 / 你的应对 / 最终结论
- `notes.md` —— 你的 patch 选题理由、踩到的坑

## 目录结构

```text
80-patches/
├── 001-dt-bindings-typo-fix/
│   ├── 0001-dt-bindings-fix-typo-xxx.patch
│   ├── notes.md
│   └── lkml-thread.md
├── 002-devm-cleanup-spi-rockchip/
│   ├── 0001-spi-rockchip-use-devm_kzalloc.patch
│   ├── cover-letter.txt
│   ├── notes.md
│   └── lkml-thread.md
└── ...
```

## 候选 patch 清单（由易到难）

| # | 选题类型 | 难度 | 接受率 | 例子 |
|---|---------|------|--------|------|
| 1 | Documentation typo | ★ | 高 | `Documentation/devicetree/bindings/` 里的拼写 |
| 2 | dt-bindings YAML 补充 | ★★ | 中 | 给 vendor 已用但未文档化的 property 补 binding |
| 3 | `devm_*` 替换 | ★★ | 中高 | 把 `kmalloc + kfree` 换成 `devm_kzalloc` |
| 4 | Coccinelle warning 修 | ★★ | 高 | 跑 `coccicheck` 看现成的小问题 |
| 5 | 修一个真 bug | ★★★ | 看运气 | 自己撞到的 + 能复现 + 能解释 |
| 6 | 新驱动 / 新硬件支持 | ★★★★ | 长周期 | 给手边某个 RK3568 板子加一个外设 |

## 提 patch 流程清单

```bash
# 1. 看历史，跟着风格走
git log --oneline drivers/path/to/file.c | head -20

# 2. 找 maintainer
./scripts/get_maintainer.pl drivers/path/to/file.c
# 输出形如：
#   Foo Maintainer <foo@kernel.org> (maintainer:SUBSYS X)
#   linux-rockchip@lists.infradead.org (open list:RK3568)
#   linux-kernel@vger.kernel.org (open list)

# 3. 生成 patch
git format-patch -1 --subject-prefix="PATCH" -o /tmp/patches

# 4. 必须 0 warning
./scripts/checkpatch.pl --strict /tmp/patches/0001-*.patch

# 5. 跑 lkp / coccicheck 自检（可选但建议）
make C=1 drivers/path/to/file.c
make coccicheck M=drivers/path/to MODE=patch

# 6. 发出去
git send-email \
  --to="Foo Maintainer <foo@kernel.org>" \
  --cc="linux-rockchip@lists.infradead.org" \
  --cc="linux-kernel@vger.kernel.org" \
  /tmp/patches/0001-*.patch
```

## 发完之后

- 24-48 小时内 maintainer 一般会回（或沉默 = 暂时没人理，1 周后温和地 ping）
- 收到 review：**当天回**，态度感谢、技术上据理力争
- 改 v2：`git format-patch -v 2 -1`，subject 自动带 `[PATCH v2]`
- 在 cover letter 写 `Changes in v2: ...`

## 铁律

- §8.3 commit message 严格按内核风格（subject < 75 字符、body 解释 why）
- §8.7 抄完一段必须能合书复述思路 —— **patch 必须自己懂**
- §8.8 永不破坏用户态 ABI
- §8.10 不要怕被 reject —— 被 reject 也是经验

## 经验帖（必读）

- ["Submitting Your First Patch" - LKML](https://www.kernel.org/doc/html/latest/process/submitting-patches.html)
- [Greg KH "How to do Linux kernel development"](https://www.youtube.com/results?search_query=greg+kh+how+to+do+linux+kernel+development)
- 看 git log 找一个最近合的小 patch，照着模仿

## 内核子系统 → mailing list 速查表

| 子系统 | 收件 list | get_maintainer 关键字 |
|--------|-----------|----------------------|
| GPIO | `linux-gpio@vger.kernel.org` | GPIO SUBSYSTEM |
| I2C | `linux-i2c@vger.kernel.org` | I2C SUBSYSTEM |
| SPI | `linux-spi@vger.kernel.org` | SPI SUBSYSTEM |
| IIO（sensor） | `linux-iio@vger.kernel.org` | IIO SUBSYSTEM |
| V4L2 / Camera / Media | `linux-media@vger.kernel.org` | MEDIA INPUT INFRASTRUCTURE |
| DRM / KMS / GPU | `dri-devel@lists.freedesktop.org` | DRM DRIVERS |
| DMA-BUF | `linux-media@vger.kernel.org` + `dri-devel@lists.freedesktop.org` | DMA BUFFER SHARING FRAMEWORK |
| IOMMU | `iommu@lists.linux.dev` | IOMMU DRIVERS |
| remoteproc | `linux-remoteproc@vger.kernel.org` | REMOTE PROCESSOR (REMOTEPROC) SUBSYSTEM |
| Pinctrl | `linux-gpio@vger.kernel.org` | PIN CONTROL SUBSYSTEM |
| Clk | `linux-clk@vger.kernel.org` | COMMON CLK FRAMEWORK |
| Regulator | `linux-kernel@vger.kernel.org` | REGULATOR FRAMEWORK |
| dt-bindings | `devicetree@vger.kernel.org` | OPEN FIRMWARE AND FLATTENED DEVICE TREE BINDINGS |
| **Rockchip 专用** | `linux-rockchip@lists.infradead.org` | ARM/Rockchip SoC support |
| **Qualcomm 专用** | `linux-arm-msm@vger.kernel.org` | ARM/QUALCOMM SUPPORT |
| 文档 | `linux-doc@vger.kernel.org` | DOCUMENTATION |
| 总是 cc | `linux-kernel@vger.kernel.org` | （兜底） |

**规则**：
- subsystem list + 平台 list（rockchip/msm）+ LKML（兜底）三件套
- maintainer 个人邮箱用 `--to=`，list 用 `--cc=`
- 永远跑 `./scripts/get_maintainer.pl <文件>` 拿权威结果，上表只做速查
