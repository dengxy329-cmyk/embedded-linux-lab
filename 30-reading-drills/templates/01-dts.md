# {Case 标题} / dts

## 设备树节点（在板子的 dts 里）

```dts
// arch/arm64/boot/dts/rockchip/rk3568-xxx.dts
&xxx {
    compatible = "rockchip,rk3568-xxx";
    reg = <0x0 0xfe000000 0x0 0x1000>;
    interrupts = <GIC_SPI 56 IRQ_TYPE_LEVEL_HIGH>;
    clocks = <&cru CLK_XXX>;
    clock-names = "xxx";
    pinctrl-0 = <&xxx_pins>;
    pinctrl-names = "default";
    status = "okay";
};
```

## 节点字段解读

| 字段 | 值 | 含义 | 驱动里怎么用 |
|------|----|----|-----------|
| `compatible` | `"rockchip,rk3568-xxx"` | 匹配 driver | `of_match_table` 里查 |
| `reg` | `<0x0 0xfe000000 0x0 0x1000>` | 寄存器物理地址 + 大小 | `devm_platform_ioremap_resource` |
| `interrupts` | `<GIC_SPI 56 IRQ_TYPE_LEVEL_HIGH>` | 中断号 + 触发方式 | `platform_get_irq` + `devm_request_irq` |
| `clocks` | `<&cru CLK_XXX>` | 时钟提供者 + ID | `devm_clk_get(dev, "xxx")` |
| `pinctrl-0` | `<&xxx_pins>` | 默认 pinctrl 状态 | 自动应用 |

## 对应的 dt-bindings YAML

```yaml
# Documentation/devicetree/bindings/.../rockchip,rk3568-xxx.yaml
$id: ...
title: ...
properties:
  compatible:
    enum:
      - rockchip,rk3568-xxx
  reg:
    maxItems: 1
  ...
required:
  - compatible
  - reg
  - ...
```

## 与父节点 / 子节点的关系

```text
soc {
    └─ xxx-controller @ 0xfe000000      ← 本节点
        ├─ child-device @ ...           ← 挂下来的设备
        └─ child-device @ ...
}
```

## mainline 节点 vs BSP 节点的差异

| 字段 | mainline | BSP | vendor 加了什么 / 为什么 |
|------|----------|-----|------------------------|
| `rockchip,xxx-quirk` | 没有 | 有 | 修复某硬件批次问题 |
| ... | ... | ... | ... |

## 链接

- mainline dts: <elixir 链接>
- BSP dts: <github 链接>
- bindings YAML: <elixir 链接>
