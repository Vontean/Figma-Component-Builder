# System Token

System Token 命名跨组件复用的用途、程度或场景语义。

## 公式

```text
{category}{SemanticRole}{VariantOrState?}
```

使用 lowerCamelCase。品牌、主题、平台和具体数值是上下文，不进入名称。

## Category 与词表

| Category | 结构 | 词表 / 示例 |
| --- | --- | --- |
| Color | `color{Role}{Variant?}` | `colorTextPrimary`、`colorAccentSecondary3` |
| Typography | `typography{Role?}{Level?}{Weight?}{Property?}` | `typographyBody1MediumFontSize` |
| Size | `size{Use}{Scale}` | `sizeControlHeightMd`、`sizeIconLg` |
| Spacing | `spacing{Scale}` | `spacingMd`、`spacing2xl` |
| Radius | `radius{Scale}` | `radiusMd`、`radiusFull` |
| Shadow | `shadow{Elevation}{Property}` | `shadowSubtleBlur`、`shadowModalColor` |
| Motion | `motion{Scene}{Action?}{Property}` | `motionLayerEnterDuration` |

### Color

仅在目标项目使用相同语义时采用以下词根：

```text
text: primary, primary2, secondary, secondary2, tertiary, and inverse forms
accent: primary, containerSubtle, containerSoft, containerStrong, containerIntense, onAccent
success / error / warning: default, containerSubtle, containerSoft, containerStrong, containerIntense
background: level1, level2, level3
neutralContainer: 0–3
dividerDefault
overlay: scrim, inverseSurface
other: use the confirmed semantic leaf
```

### Typography

```text
role: title, body, subheadline, caption
title level: large, 1–4
weight: regular, medium, semibold, bold
property: FontFamily, FontStyle, FontSize, LineHeight, LetterSpacing, ParagraphSpacing
```

Typography Variables 命名 recipe 的一个属性。任务要命名或创建完整 typography treatment 时，按本规则命名属性 Variables，并使用对应 Figma Text Style 作为 recipe consumer；不把单个属性 Variable 当作完整 typography token。

### Size、Spacing、Radius、Shadow

```text
size use: controlHeight, icon, border
scale: 3xs, 2xs, xs, sm, md, lg, xl, 2xl, 3xl, 4xl
inserted scale: {scale}Plus
border: none, hairline, thin, thick
radius: none, 2xs, xs, sm, md, lg, xl, full
shadow: none, subtle, raised, floating, overlay, modal
shadow property: Color, OffsetX, OffsetY, Blur, Spread
```

Shadow Variables 命名 recipe 的一个属性。使用对应 Figma Effect Style 作为完整 elevation recipe 的 consumer。

### Motion

```text
scene: feedback, state, layer, page, indicator
property: duration, easing, transition
```

在 scene 与 property 之间，只保留用于区分 recipe 的 action，例如 `Enter`、`Exit`、`Click`。

## 构词规则

- 用语义角色命名，不用组件名或 resolved value。
- 同一语义在不同品牌或 mode 下保持同一个公开名。
- 选择能够跨组件成立的最短角色词；只服务单个组件的词进入 Component。
