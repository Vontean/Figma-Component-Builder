# Component Token

Component Token 命名单个组件内的设计职责。

## 公式

```text
{component}{Scope?}{State?}{Anatomy?}{Property}
```

使用 lowerCamelCase。`component` 与 `property` 必填。

```text
selectorPrimaryContainerColor
selectorPrimaryDisabledContainerColor
selectorPanelIconTextSpace
selectorGroupBetweenSpace
```

## 词根

### Scope

可包含：

```text
color scheme: primary, secondary, tertiary, danger
variant: panel, inCard, compact, circular
composition: group
component-specific: progress, loading, or another confirmed variant
```

只保留会改变该 token 含义的 Scope。词表是构词参考，不替代目标组件已确认的 variant 名称。

### State

```text
common: disabled, hover, pressed
input: focus
toggle: checked, unchecked
selection: selected, unselected
```

默认态 `normal` 省略。State 出现时放在 Anatomy 前，例如 `selectorPrimaryDisabledTextColor`。

### Anatomy

使用目标组件已确认的 Anatomy 名称。缺少必要名称时，只询问确认该名称所需的最小问题，不另造同义词。

```text
container, text, icon, indicator, outline, listItem, stateLayer
supportingText, inputText, title, subtitle
handle, track, thumb, divider, separator, caret
```

同一 Property 可能作用于多个部位时保留 Anatomy。通用文字部位使用 `text`；描边部位使用 `outline`。上列仅为常见 Anatomy，不是封闭词表。

### Property

| 类型 | 结构 / 词表 |
| --- | --- |
| Color | `containerColor`、`textColor`、`iconColor`、`outlineColor`、`indicatorColor` |
| Space | `leadingSpace`、`trailingSpace`、`topSpace`、`bottomSpace`、`betweenSpace` |
| Different-element space | `{elementA}{elementB}Space`，例如 `iconTextSpace` |
| Size | `containerSize`、`containerHeight`、`containerWidth`、`containerMinHeight`、`containerMaxWidth`、`iconSize`、`outlineWidth` |
| Radius | `containerRadius`，或带明确方位的 corner radius |
| Font | `textFont` |
| Shadow | `shadow` |
| Motion | `motion`、`pressMotion`、`hoverMotion` |

相同元素之间使用 `betweenSpace`；不同元素直接按 Anatomy 顺序拼接。间距统一以 `Space` 结尾。

## 最短充分

- 省略后仍能唯一表达同一职责的 Scope、State 或 Anatomy，应省略。
- 省略后会混淆 Variant、状态或部位的词根，应保留。
- 使用组件语义词，不使用品牌、主题、平台或具体数值。
