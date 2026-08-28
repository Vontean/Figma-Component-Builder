# Anatomy

定义图层命名与结构实现，让图层、`Component Token` 和实例行为互相可查。

## 建层前的结构决策

创建或替换图层前，按每个变体确定：

- 会受父级尺寸影响的图层及其响应意图。
- 可交换子组件及其默认实例。
- 值得抽取为私有部件的候选。
- 图层的语义层级顺序。

这些是 Anatomy 的一部分；不以完成视觉后再补结构为默认顺序。

## 规则

1. **表意为先**：图层名让读者一眼判断语义，例如 `icon`、`text`、`container`、`supporting text`。
2. **与 token 互为索引**：图层名与对应 token 的 Anatomy 词根一致；token 能反查图层，图层能反查 token。
3. **语义优先例外**：token 词根不清晰或组件语境更自然时，以清晰表达设计语义为准。

## 从 token 拆图层

按 `Component Token` 公开名的 Anatomy 词根映射图层名；完整词根表见 `vt-token-naming/references/component-tokens.md`。`Between` 类 token 对应的两个图层必须存在且相邻。

## 尺寸响应

先记录图层相对于父级的尺寸和锚点意图，再选择相应的 Figma layout 或 constraints。下表是常见映射，不是按图层名套用的规则：

| 响应意图 | 常见 constraints |
| --- | --- |
| 横向随父级变化、保持固有高度 | `STRETCH / MIN` |
| 固定尺寸并固定在起点 | `MIN / MIN` |
| 横向随父级变化且垂直居中 | `STRETCH / CENTER` |

父级使用 Auto Layout 时，以 sizing 和对齐实现同一意图，不机械套用 constraints。

## 可交换子组件

将需要 `INSTANCE_SWAP` 的子组件作为 Anatomy 输入：确定目标实例、默认实例、在父组件中的位置，以及是否需要独立的可见性控制。由 `variant-properties.md` 决定它是否作为实例作者的公开 API 和可用候选。

目标实例可以直接绑定 Instance Swap。仅当该位置需要独立的尺寸、constraints、定位、裁切或可见性行为时，才增加包裹 Frame；该 Frame 也必须命名并纳入 Anatomy。

## 私有部件与层级顺序

同一职责、结构和 token/Style 绑定在多个变体重复时，可作为私有部件候选；仅尺寸不同不构成抽取理由。是否创建和发布该部件按 `component-organization.md` 决定。

z-order 是语义结构。替换或插入图层时保持该变体既定的层级位置；层级变化须作为明确的结构决策。

## 验证

- 每个图层名可对应一个 token Anatomy 词根，或有语义优先例外的明确理由。
- 受父级尺寸影响的图层符合已确认的响应意图；各变体的差异明确。
- 每个 Instance Swap 实际关联目标实例；使用包裹 Frame 时，该 Frame 的职责与行为明确。
- 替换或插入后的图层顺序符合该变体的语义层级。
