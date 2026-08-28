# Token 绑定

定义组件的 token 层级、`Component Token` 矩阵与绑定决策。Variables 创建 API 语法见官方 `figma-generate-library`；公开名与 slash path 见 `vt-token-naming`。

## 绑定契约

按当前项目已确认的层级绑定组件视觉图层。常见链路为：

`Reference → System → Component → 组件内部图层`

- System Token 是业务设计使用的公共 token surface；Component Token 是组件的内部职责层。是否允许组件直接绑定 Reference Token，以项目 contract 为准。
- Variant / Boolean / Text / Instance Swap 是组件配置接口：它们选择场景或内容；对应内部图层仍绑定其视觉职责 token 或 Style。
- Component Token 的 scope 是暴露边界，不是权限控制。实现型 token 通常设为 `[]`；以项目既有 scope 规则为准。

## Component Token 矩阵

设计师提供组件 token 矩阵时，以其为 source of truth：保留既有列结构，不新增用于解释复用理由或绑定目标的列。这些理由在 agent 回报中逐行说明。

同一 Component Token 可绑定多个同职责图层；相同 System alias 不自动代表同一 Component Token。只有组件职责也相同时才复用同一 Component Token。

## 最小充分语义面

用**语义粒度控制**避免 token 增殖：仅在以下任一情况新建 Component Token。

- 表达一个独立的组件职责。
- 需要在变体、状态或 Anatomy 间独立演进。
- 预期未来需改绑到不同的 System Token。

仅因图层不同、当前数值不同，或某个 Variant 存在，都不足以新建 token。先在矩阵中寻找同职责的既有 Component Token；无法复用时才新增。

## 迁移分支

### 已有矩阵

1. 读取矩阵、组件变体和 Anatomy，逐行核对绑定目标。
2. 用 `vt-token-naming` 校验或补齐 Component Token 名称与 slash path。
3. 创建或复用矩阵列出的 Component Token，按项目规则 alias 到目标 token，并核对所有 mode。
4. 将每行绑定到正确的组件 variant / 状态 / 图层属性。
5. 回读 Variables 与组件绑定，确认矩阵每行均已实现。

### 未有矩阵

先读取目标组件的 variant、状态、Anatomy，以及用户指定的旧组件或已有 Guideline 信息；据此按项目既有列结构生成**候选矩阵**。复用或新建理由、绑定目标在 agent 回报中逐行说明。得到设计师确认前，不创建 Component Token，也不更改组件绑定；确认后再执行 token 创建和绑定。

## 局部常量

- **受控例外**：仅当值是组件局部常量、没有跨组件语义且没有合适的 System Token 时，Component Token 可保留 raw value。
- raw value 仍必须通过 Component Token 绑定，不能直接写入组件图层。
- 执行完成时，向设计师单列所有 raw value：Component Token、值、绑定属性、保留 raw 的理由，以及是否值得后续提升为 System Token。

## Recipe 与 Style

**Recipe** 是在一个使用场景中必须成组取值的 System Token 组合；它不是 Figma Variable 的数据类型。Component Token 的 `STRING` 值仅记录 recipe 引用，不能将 recipe 直接绑定到图层。

- **Typography recipe**：System Typography 的原子 Variables 绑定到一个 Text Style。组件图层绑定该 Text Style（`textStyleId`）；Component Token 的 recipe `STRING` 记录能唯一指向该 Style 的项目 identifier。
- **Shadow recipe**：System Shadow 的颜色、offset、blur、spread 等原子 Variables 绑定到一个 Effect Style。组件图层绑定该 Effect Style（`effectStyleId`）；recipe identifier 与 Effect Style 展示名可以使用不同命名格式。
- **Motion recipe**：System Motion 可记录 duration、easing、transition 等成组语义，但 Figma 没有 Motion Style，也没有可将 `STRING` recipe 绑定到动画的属性。Component Token 的 `STRING` 值只作为 recipe 引用与交付信息，不宣称已产生 Figma 绑定。

创建或迁移 recipe 时，先用 recipe identifier 在当前文件的已有 Style 及其 System Token 绑定中解析唯一目标，再对图层应用对应 Style；回读 Style 名称、Style ID 和其原子 System Token 绑定。无法唯一解析时，回报并等待设计师指定映射。

## 属性与 Mode

- 颜色、间距、圆角、尺寸、描边等单值视觉属性，按矩阵绑定对应类型的 Component Token；Typography 与 Shadow recipe 按上节绑定 Style。
- `lineHeight` 默认由 Text Style 的 `AUTO` 承载；仅在矩阵明确要求时才单独处理。
- 绑定前读取 collection 的全部 mode 与 `valuesByMode`，不能只按默认 mode 判断 alias 是否完整。
- 各 collection 的 mode 和命名以目标文件实测为准。
