# 无障碍 Annotation

组件 Guideline 中的 Annotation 板块，用来说明焦点朗读顺序与焦点标注属性。组件 Set 的 Description 只保存指向该板块的 URL；字段写法见 [component-description.md](component-description.md)。

## 板块职责

- 每个案例说明辅助技术抵达元素的顺序，以及每个焦点对象的标注属性。
- 视觉示例、焦点顺序与标注卡片共同构成可审阅的 Guideline；不把这些内容压缩为 Set Description 文本。
- 项目采用 Accessibility 链接字段时，其 value 直接链接到此板块根 Frame。

## Agent 可读结构

板块根 Frame 命名按项目既有术语确定。使用浅层、语义化的层级，让 agent 先读结构，再按需读案例内容：

```text
Annotation
├─ Intro
└─ Cases
   └─ {Scenario}
      ├─ Example
      ├─ Focus order
      └─ Annotation cards
```

- `Intro` 说明该组件的无障碍标注目的。
- 每个 `{Scenario}` 将示例、焦点顺序和标注卡片放在同一命名容器中；案例从上到下排列。
- 用实际语义命名根 Frame、案例和内容容器；`Group`、`Frame`、`Rectangle` 等默认名不承担结构语义。
- 隐藏草稿、过期示例与无关节点不放在板块根 Frame 内，避免读取时消耗无效上下文。
