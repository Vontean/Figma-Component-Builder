# Component Description

为组件写入无法用 `Component Property` 表达的补充信息。它只提供简短的使用语境、特殊交互和 Guideline 链接；完整使用说明仍属于项目 Guideline。

## 执行

### 1. 确认目标与链接

- 默认写入 `COMPONENT_SET`；没有 Component Set 的独立 `COMPONENT` 才写入该组件。不要写入 Set 内的 variant、实例或页面。
- 读取当前项目的 Description contract、Guideline 中的使用原则或交互规则，以及本次确实存在且与组件相关的文档板块。
- 已确认的组件 Guideline 页面或板块链接优先从 `@data/links.csv` 读取；没有记录时使用本次任务提供或确认的链接。
- `documentationLinks` 是否使用、指向页面根节点还是其他目标，以项目 contract 为准。

### 2. 组织内容

按当前项目 contract 选择区块与顺序。只加入当前 Guideline 确实存在且与组件相关的板块；`Measurements`、`Motion` 等不是固定必备区块。一个常见 contract 是 `Scenario` → `Note` → `Accessibility` → `Responsive`：

| 区块 | 写入条件 | 内容 |
| --- | --- | --- |
| `Scenario` | 需要简短使用说明。 | 说明何时使用；同一家族有多个 type 时，写清各自触发条件和相邻类型的选择边界。内容从已确认 Guideline 的使用原则或交互规则提炼。 |
| `Note` | 有 Property 无法表达的特殊交互。 | 该交互行为。 |
| `Accessibility` | 对应 Guideline 板块已存在。 | Annotation、Dynamic Font 等精确链接。 |
| `Responsive` | 对应 Guideline 板块已存在。 | Layout 等精确链接。 |

没有补充信息时，Description 可以为空；不要为了凑齐区块写入无效内容。

```md
## Scenario

简短使用说明

## Note

无法通过属性表达的特殊交互行为

## Accessibility

Annotation: [Guideline - Annotation](https://example.com/annotation)
Dynamic Font: [Guideline - Dynamic Font](https://example.com/dynamic-font)

## Responsive

Layout: [Guideline - Responsive](https://example.com/responsive)
```

### 3. 写入富文本

| 属性 | 用途 | 写入规则 |
| --- | --- | --- |
| `description` | 无格式纯文本。 | 审计时读取，不以它写入富文本。 |
| `descriptionMarkdown` | Figma UI 渲染的富文本。 | 写入本页组织好的 Markdown。 |

写入前必须使用 `figma.util.normalizeMarkdown(md)`。`documentationLinks` 当前只支持一个 URL。

```js
const md = `## Scenario

用于调节**连续数值**。

## Accessibility

Annotation: [Guideline - Annotation](https://example.com/annotation)
`;

target.descriptionMarkdown = figma.util.normalizeMarkdown(md);
target.documentationLinks = [{
  uri: 'https://example.com/component-guideline'
}];
```

### 4. 验证

回读目标节点，确认：

- 目标类型正确，字段顺序符合项目 contract。
- `descriptionMarkdown` 已保存为规范化后的 Markdown；Scenario 可区分同一家族各 type 的使用时机，所有链接均来自实际读取并确认的页面或 section。
- 使用 `documentationLinks` 时，其链接指向已确认页面或目标节点。
- UI 未显示或未更新时，重新 publish 目标节点后复查。

完成条件：Description 只含本组件的补充信息，富文本与 Guideline 链接均可读且可跳转。

## Figma Document 富文本语法

| 格式 | 语法 |
| --- | --- |
| 二级标题 | `## 标题` |
| 加粗 | `**text**` 或 `__text__` |
| 斜体 | `*text*` 或 `_text_` |
| 删除线 | `~~text~~` |
| 行内代码 | `` `code` `` |
| 代码块 | 三个反引号包裹代码 |
| 链接 | `[text](url)` |
| 无序列表 | `- item` 或 `* item` |
| 有序列表 | `1. item` |
| 段落 | 用一个空行分隔 |

- 标题只使用 `##`；`#` 会降级为 `##`。
- 代码块内不使用其他 Markdown 格式。

## 关联 reference

- Accessibility Annotation 的目标板块与可读结构见 [ada-annotation.md](ada-annotation.md)。
- Dynamic Font 的目标板块与可读结构见 [ada-dynamic-font.md](ada-dynamic-font.md)。
- Responsive Layout 的目标板块与设备尺寸适配约束见 [responsive-design.md](responsive-design.md)。
