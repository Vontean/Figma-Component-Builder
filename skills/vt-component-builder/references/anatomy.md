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

1. **设计师可读**：使用普通的英文表述，不用考虑代码形式，方便设计师看懂，例如 `Icon`、`Supporting text`、`Container`。
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

私有部件名称使用 `._XXXX` 格式：`._` 是固定前缀，`XXXX` 使用本页定义的普通英文语义名。平台、类型或变体后缀按目标文件已经确认的命名方式处理，不从单个组件特例推导通用格式。

z-order 是语义结构。替换或插入图层时保持该变体既定的层级位置；层级变化须作为明确的结构决策。

## 结构重组与样板传播

重组父子结构前，记录受影响节点的层级、尺寸、Auto Layout 上下文、sizing/grow、padding/spacing、Variable/Style 绑定和 Component Property 引用。Reparent 会改变布局上下文，并可能引起尺寸或位置变化。

多个变体应具有相同结构时，可将一个已确认变体作为样板。先逐项 diff 各变体的结构、绑定和属性引用，确认差异只来自变体内容，再 clone 或传播样板结构；传播后恢复变体特有内容与属性引用。

## 验证

- 每个图层名可对应一个 token Anatomy 词根，或有语义优先例外的明确理由。
- 受父级尺寸影响的图层符合已确认的响应意图；各变体的差异明确。
- 每个 Instance Swap 实际关联目标实例；使用包裹 Frame 时，该 Frame 的职责与行为明确。
- 替换或插入后的图层顺序符合该变体的语义层级。
- 私有部件使用 `._XXXX` 前缀，语义名对设计师可读。
- 结构重组后的所有受影响变体，其宽高、层级、绑定、属性引用和截图均符合预期；每项差异均能对应已确认的变体差异。
