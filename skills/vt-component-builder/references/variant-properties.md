# 变体属性

定义 `Component Set` 的公开 API：用户在组件 Instance 中可选择什么，以及选择应使用 Variant、Text、Boolean 还是 Instance Swap。组件内部 token、图层实现与响应式规则不暴露为 `Component Property`。

## 先定 Component Set 边界

先判断候选内容是否仍是同一个 Component Set。只有同一使用场景、核心 Anatomy 与公开 API 的不同呈现，才在同一个 Set 中用 property 表达。若它们不同，提出拆分为独立 Component Set 的建议，再设计 property。

## 选择 Property 类型

| 类型 | 适用的实例作者选择 |
| --- | --- |
| `TEXT` | 可独立编辑的内容。 |
| `BOOLEAN` | 同一 Anatomy 中可独立出现或隐藏的元素。 |
| `INSTANCE_SWAP` | 从受控 Building Block 集合替换的子组件。 |
| `VARIANT` | 有限且互斥、会改变组件呈现或行为状态的选择。 |

- Property 名描述实例作者的选择，例如 `Show icon`、`Style`、`State`；不使用图层名、Component Token 或视觉属性名。
- 一个选择若只服务组件内部实现，不建立 Component Property。
- 图标或其他可替换 Building Block 优先用 `INSTANCE_SWAP`，不为每个候选项扩充 Variant 值。

## Instance Swap 的结构输入

先读取 Anatomy 已确认的目标实例、默认实例、位置和可见性需求；本文件再决定是否将其暴露为实例作者的选择。

- Property 名描述实例作者要替换的对象，不描述其容器或图层实现。
- 需要受控候选时，按已确认的候选组件设置 Preferred Values；不从旧组件或名称相近对象推断候选。
- 仅服务内部结构的替换不建立公开 `INSTANCE_SWAP` Property。

## 有效组合

先列出有效的 Variant 组合，再创建变体。各轴只有能独立组合时才分开；互斥或强耦合的选择应收敛为一个值、拆分 Component Set，或留给上层组合组件。不得为不存在的使用场景生成变体。

每个 Property 都必须有有效默认值，并在实例中产生可见、可预测的结果。

## 旧组件改造

旧 Component Set 的 property 逻辑是改造的起点，不是新库规范。先读取旧 Set、其变体与子图层，可以向设计师输出 property 差异：

- **保留**：仍是实例作者需要的选择。
- **收敛**：重复或强耦合的选择合并。
- **替换**：改用更合适的 Property 类型，例如图标 Variant 改为 `INSTANCE_SWAP`。
- **移除**：仅服务旧图层实现、token 或设备示例的属性。
- **新增**：新组件使用场景确实需要、旧 API 未覆盖的选择。

设计师确认该差异前，不重建 Component Set 或更改 property。

## 验证

- 公开 API 只包含实例作者可理解和可操作的选择。
- 每个值都有对应且唯一的有效呈现；没有孤立 variant 或无效组合。
- 每个 `TEXT`、`BOOLEAN`、`INSTANCE_SWAP` 都实际作用于目标子图层或子组件。
- 创建后使用 Figma 返回的 property key 校验绑定，不猜测 key。
