---
name: vt-component-builder
description: 为 Figma 组件库搭建或迁移组件，补充 token、结构、公开 API 与文档契约的特定操作 context。
disable-model-invocation: true
---

# VT Component Builder

为 Figma 组件库搭建或迁移组件。实际操作使用官方 `figma-use`；通用搭建与验证方法参考官方 `figma-generate-library`。本技能定义组件的 token、结构、公开 API 与文档决策。

## 与官方 Figma skills 协作

每次实际调用 `use_figma` 前，读取 `figma-use`，并在 `skillNames` 中包含它。

- 任务明确包含 foundation、文件级文档或完整库流程时，额外读取 `figma-generate-library` 并在 `skillNames` 中加入它；从 MCP resource 加载官方 skill 时，按官方规则使用 `resource:` 前缀。
- 调用 token 命名或读写 Figma Variables 时，读取 `vt-token-naming`。

本技能只交付本次确认范围内的对象。官方基础文档、组件页文档、token 矩阵和使用说明都是独立范围：没有明确授权时只读取，不创建或维护。

## 项目数据

1. 任务需要已确认的文件或页面链接时，先读取 `@data/links.csv`；表格使用 `key,url` 两列。
2. 没有对应记录时，使用本次任务提供的链接，不猜测链接。
3. 用户提供或确认稳定链接后，创建 `data/`（如不存在）并新增或更新对应行。
4. 只保存项目导航链接，不保存凭证、导出内容或敏感 token 数据。

## 项目文件职责

先读取当前项目的文件职责与本次写入授权。常见分工是：组件库文件负责组件、Variables 与 Styles；组件 Guideline 文件负责 token 矩阵和使用文档。实际分工、文件链接和页面位置以项目现状为准，可从 `@data/links.csv` 读取已确认入口。

文件职责不等于默认写入授权。用户提供组件 token 矩阵时，先读取并按其实施；矩阵创建/更新、组件页文档和使用文档是彼此独立的显式范围。

## 核心约束

- 组件视觉图层按项目已确认的 token 层级绑定；详见 [token-binding.md](references/token-binding.md)。
- `Component Set` 的公开 API 只暴露实例作者的选择；内部图层、token 与响应式实现不作为 property 暴露。
- 历史组件和既有 Guideline 是迁移输入，不是新组件的默认规范。只有用户指定的参照或已确认的矩阵可作为设计依据。
- 组件、页面和 property 命名匹配目标文件；Variable 公开名和 slash path 通过 `vt-token-naming` 双向校验。

## 四阶段 workflow

### 1. Discovery 与范围确认（只读）

- 已提供矩阵链接或位置时，只读取矩阵、目标 `Component Set`/变体/Anatomy、矩阵涉及的 Variables 和 Styles，以及本次范围明确要求的文档板块。
- 未提供矩阵时，读取目标组件、Variables、Styles、页面组织和既有命名；任务包含完整库流程时，再按官方方法完成库资源发现。
- 无法定位已确认矩阵时，依据目标组件的使用场景、状态和 Anatomy 提出候选矩阵；不以其他组件的实现推断规则。
- 创建或重构图层前，确定 Anatomy 的结构决策：尺寸响应意图、可交换子组件、可抽取私有部件候选，以及各变体的语义层级顺序。
- 输出组件范围、Token/Style 复用或新增项、矩阵状态、文档写入范围和待决策差异；矩阵与组件现状冲突且不同选择会改变实施结果时，使用 `request_user_input` 让设计师选择。

完成条件：组件范围和迁移边界明确；矩阵每行均已归类为可执行或显式排除，所有冲突均已得到设计确认；或候选矩阵已获设计确认。

### 2. 矩阵与基础

- 仅当任务明确要求，且设计师已确认矩阵不存在或需更新时，才创建或维护该组件的 token 矩阵；保留项目既有的列结构。
- 在组件库文件创建或复用矩阵所需的 `Component Token`，并按项目约定建立 alias；Typography 与 Shadow recipe 绑定相应 Style。
- 对矩阵与 Variables 执行 reference 定义的命名、mode、scope、raw value 例外与回读验证；每个新建或修改的 Variable 必须通过 `vt-token-naming` 的 Variable 完整性门禁。

完成条件：矩阵和 Token/Style 的对应关系可追溯；本次新建或修改的每个 Variable 已通过 Variable 完整性门禁；本次组件所需基础已验证。

### 3. 组件搭建

- 逐个 `Component Set` 在目标家族页创建、重建或扩展；没有页面时，先确认项目的页面组织方式。
- 按确认的 Anatomy 结构决策和关联 reference 创建图层、公开 API、组件角色和交互热区；constraints、swap、部件抽取和私有部件命名在建层时完成。
- 将对应 `Component Token` 或 Style 绑定到图层；建立 `Component Property` 与子图层/实例的实际关联。

完成条件：每个目标 `Component Set` 的 variant、公开 API、结构、组件源与范围内实例的绑定均已验证。

### 4. 文档与验证

- 仅在本次范围包含时，创建或整理 Accessibility、Dynamic Font、Responsive Layout 等文档板块，并使用对应 reference 的 agent 可读结构。
- 每个写入批次回读受影响对象；结束时回读本次范围的矩阵、Variables、Styles、组件绑定、`Component Set` description 与文档链接，并完成 metadata 与截图验证。
- 回报所有 `Component Token` raw value、无法唯一解析的 recipe/Style、未解决差异和后续设计决策。

完成条件：范围内文件的角色和链接一致；所有例外与未决项已显式回报。

## Reference 路由（按需读取）

不扫描整个 `references/` 目录；仅在下列触发条件成立时读取对应文件。

| 主题 | 读取时机 |
| --- | --- |
| [token-binding.md](references/token-binding.md) | 创建、迁移或绑定 `Component Token`、Style 或 token 矩阵 |
| [hit-area.md](references/hit-area.md) | 创建交互组件或审计热区 |
| [component-description.md](references/component-description.md) | 写入或审计 `Component Set` 或独立 `Component` 的 Description |
| [ada-annotation.md](references/ada-annotation.md) | 创建、整理或链接 Annotation 板块 |
| [ada-dynamic-font.md](references/ada-dynamic-font.md) | 创建、整理或链接 Dynamic Font 板块 |
| [responsive-design.md](references/responsive-design.md) | 处理设备/可用空间适配或 Responsive Layout 板块 |
| [anatomy.md](references/anatomy.md) | 定义或迁移图层结构、命名、尺寸响应、可交换子组件、私有部件或层级顺序 |
| [variant-properties.md](references/variant-properties.md) | 设计或改造 `Component Set` 边界和公开 API |
| [component-organization.md](references/component-organization.md) | 组织家族页、公开组件、Building Block 或私有实现 |

## 交付

- 列出本次实际改动的组件、Variables/Styles、token 矩阵或文档板块。
- 回报验证证据、`Component Token` raw value 与原因、未解决的映射或设计决策。
