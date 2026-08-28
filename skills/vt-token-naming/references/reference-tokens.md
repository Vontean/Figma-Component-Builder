# Reference Token

Reference Token 命名品牌基础值、刻度或程度，不表达 UI 用途。

## 公式

```text
{brand}_{category}_{scale-or-attribute}
```

使用小写 snake_case。品牌词以当前项目已确认的 mode 或品牌命名为准；以下仅为虚构示例：`northstar`、`orion`、`lumen`。

## 结构与词表

| 类型 | 结构 | 词表 / 示例 |
| --- | --- | --- |
| Color | `{brand}_{family}_{step}` | `northstar_grey_10` |
| Font size | `{brand}_font_size_{value}` | `northstar_font_size_16` |
| Font weight | `{brand}_font_weight_{weight}` | `regular`、`medium`、`semibold`、`bold` |
| Size | `{brand}_size_{value}` | `northstar_size_16`、`northstar_size_0_5` |
| Spacing | `{brand}_spacing_{value}` | `northstar_spacing_12` |
| Radius | `{brand}_radius_{value}` | `northstar_radius_8`、`northstar_radius_full` |
| Motion | `{brand}_motion_{kind}_{attribute}` | `northstar_motion_duration_quick` |

Color family：

```text
brand, black, white, grey, bluegrey
skyblue, green, lightgreen, red, orangered, orange, yellow, indigo, purple
label, other
```

Motion kind 与常用 attribute：

```text
duration: instant, fast, quick, normal, emphasis, slow, deliberate, extra_slow, indicator_spin
curve: ease_in, ease_out, ease_in_out, linear, swift_out
dynamics: spring, panel_spring_out
effect: fade, scale, slide, position, size, tone
```

## 构词规则

- 使用实际数值作为 Size、Spacing、Radius 和 Font size 的 identity；小数点写成 `_`，例如 `0_5`。
- 合并 Color family 中的空格词，例如 `blue grey` → `bluegrey`。
- 使用程度词命名 Motion 基础值；场景词属于 System Motion。
- 将 UI 用途放到 System，将组件局部用途放到 Component。
- Reference 不包含 Shadow。
