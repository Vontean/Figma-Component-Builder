# 无障碍 Dynamic Font

Dynamic Font 是系统字体放大下的无障碍适配。它说明文字放大后组件如何保留可读性、换行和必要的布局切换；组件 Set 的 Description 只保存指向该板块的 URL。

## 板块职责

- 说明系统字体缩放时，文字、容器和布局分别如何变化。
- 明确何时保持同一布局，何时切换布局；具体倍率与阈值由当前项目 Guideline 定义。
- 项目采用 Accessibility 链接字段时，Dynamic Font value 直接链接到此板块根 Frame；字段写法见 [component-description.md](component-description.md)。

## Agent 可读结构

板块根 Frame 命名按项目既有术语确定。使用浅层、语义化的层级：

```text
Dynamic Font
├─ Rules
└─ Cases
   └─ {Scenario}
      ├─ Default scale
      ├─ Enlarged scales
      └─ Layout switch
```

- `Rules` 记录间距、文字、容器和布局的全局关系。
- 每个 `{Scenario}` 并列展示默认、放大及布局切换状态；状态名使用板块内实际倍率，不在组件或本 reference 中编造阈值。
- 用实际语义命名根 Frame、案例和状态容器；隐藏草稿、过期示例与无关节点不放在板块根 Frame 内。
