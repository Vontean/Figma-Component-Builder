# Figma Variables 与公开 Token

仅在任务读取或创建 Figma Variables 时使用本文件。公开名与 slash path 必须能双向转换。

```text
Collection → layer
Mode → brand or theme value context
Slash path ↔ public name
```

无法唯一还原 segment 或组织 Group 时，先询问，不猜测。读取时保留文件已有 path；创建时读取同一 token family 的既有组织，或要求设计师确认。

## Variable 完整性门禁

每个本次创建或修改的 Variable，在写入后、交付前逐项回读确认：

1. 公开名与 stored slash path 通过本文件的双向回环校验。
2. `resolvedType` 与该 token 的设计职责匹配。
3. 每个 mode 都有正确的 value 或 alias；alias 的目标存在且类型匹配。
4. `description` 同时包含中文和英文，并准确说明该 token 的职责。
5. `codeSyntax` 的 `WEB`、`ANDROID`、`iOS` 均已填写，并遵循目标文件同类 token 的既有语法。
6. `scopes` 与 token 类型、组件职责及目标文件既有 scope 规则一致。
7. 回读 Variable 的名称、path、type、全部 mode value/alias、description、三端 codeSyntax 与 scopes；每一项均与写入意图一致。

完成条件：本次范围内每个 Variable 的七项均已回读通过；任一字段缺少项目约定时，先读取同类 Variable，仍无法确定则请设计师确认。

## Reference

Brand 通常来自 mode。公开名去掉 brand 前缀后的部分是 foundation leaf；slash path 的上层 segment 只负责组织。

| Slash path | Mode | Public name |
| --- | --- | --- |
| `Color/brand/brand_0` | Northstar | `northstar_brand_0` |
| `Color/blue grey/bluegrey_10` | Orion | `orion_bluegrey_10` |
| `Typography/font-size/font_size_16` | Northstar | `northstar_font_size_16` |
| `Size/size_16` | Northstar | `northstar_size_16` |
| `Spacing/spacing_12` | Northstar | `northstar_spacing_12` |
| `Radius/radius_8` | Northstar | `northstar_radius_8` |
| `Motion/duration/motion_duration_quick` | Northstar | `northstar_motion_duration_quick` |

双向规则：

```text
Color:      Color/{display-family}/{family}_{step}
Typography: Typography/{font-size|font-weight}/{font_attribute_value}
Size:       Size/size_{value}
Spacing:    Spacing/spacing_{value}
Radius:     Radius/radius_{value-or-full}
Motion:     Motion/{kind}/motion_{kind}_{attribute}
```

Color 的 display family 保留可读空格：`bluegrey ↔ blue grey`、`skyblue ↔ sky blue`、`lightgreen ↔ light green`、`orangered ↔ orange red`。

## System

System Variables 可按设计属性分 collection。slash path 的最后一段保留完整 lowerCamelCase 公开名；上层 segment 只用于当前 Figma 文件中的组织。不要把最后一段缩短为 role、scale 或 property。

下表是组织模式的示例；实际 collection 与路径以目标文件为准。

| Collection | Stored slash path |
| --- | --- |
| `System Color` | `color/{role}/{publicName}` |
| `System Radius` | `radius/{publicName}` |
| `System Spacing` | `spacing/{publicName}` |
| `System Size` | `size/{use}/{publicName}` |
| `System Motion` | `motion/{scene}/{action}/{publicName}` |
| `System Typography` | `typography/family/{publicName}` 或 `typography/{role}/{level?}/{weight?}/{publicName}` |
| `System Shadow` | `shadow/{elevation}/{publicName}` |

```text
color/text/colorTextPrimary                     ↔ colorTextPrimary
spacing/spacing2xsPlus                          ↔ spacing2xsPlus
size/icon/sizeIconMd                            ↔ sizeIconMd
motion/layer/enter/motionLayerEnterDuration     ↔ motionLayerEnterDuration
typography/body/1/medium/typographyBody1MediumFontSize
                                                ↔ typographyBody1MediumFontSize
shadow/subtle/shadowSubtleBlur                  ↔ shadowSubtleBlur
```

## Component

Component slash path 跟随同一组件的既有组织。常见组织 Group 包括 `Color`、`Shape`、`Motion`、`Group`、`Spacing`、`Typography`、`Layout` 和 `Size`。

- 读取：保留精确 stored slash path，并取最后一段作为公开 Component Token 名称。
- 创建：先读取同一组件既有 Variables 并沿用其组织；组件没有既有组织时，要求设计师确认。
- 不从公开名单独推断组织 Group。

```text
selector/Color/Primary/selectorPrimaryContainerColor
selector/Shape/Panel/selectorPanelContainerMinHeight
text/Typography/textPrimaryFont
```

## 回环校验

交付前分别执行：

```text
slash path → public name → slash path
public name → slash path → public name
```

两次结果都必须回到同一个已存储或已确认的 path；否则停止并确认缺失的组织 segment。
