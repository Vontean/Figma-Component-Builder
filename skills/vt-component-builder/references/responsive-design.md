# 响应式设计

组件搭建时如何承载设备尺寸与可用空间变化。Dynamic Font 属于无障碍适配，见 [ada-dynamic-font.md](ada-dynamic-font.md)。断点具体数值以当前项目的 Responsive Layout Guideline 为准，不在本文件编造。

## Discovery

1. 读取目标组件对应的 Responsive Layout Guideline，记录设备或可用空间状态、约束值与布局变化。
2. 在创建前锁定本次要覆盖的状态与差异；Guideline 未定义时，向用户报告缺口，不从其他组件推断。

## 组件决策

- 只把 Guideline 已定义的设备或可用空间状态加入组件 API。
- 断点值、尺寸范围与 Measurements 来自目标 Guideline，不自行设计。
- 若响应式差异是否应成为 variant、`Component Property` 或独立 `Component Set` 没有明确来源，先呈现差异和取舍，等待用户决定。

## Responsive Layout Guideline 链接

设备尺寸、横竖屏、可用宽高与布局切换说明属于项目 Guideline，不复制到组件库文件。需要补充该说明时：

1. Guideline 板块根 Frame 使用项目确认的语义名称，并以定义、规则与案例组织。
2. 案例按组件场景分组，并列展示实际设备或可用空间状态、布局规则与 Measurements；具体设备、阈值与尺寸以该板块内容为准。
3. 项目 Description contract 包含 Responsive 链接时，将该根 Frame 的精确 URL 写入指定字段；格式见 [component-description.md](component-description.md)。
4. 任务明确包含 Responsive Layout 文档时，按本文件的结构创建或整理该板块；否则只读取和验证链接。

## Agent 可读结构

```text
Responsive Layout
├─ Definition
├─ Rules
└─ Cases
   └─ {Scenario}
      ├─ Device states
      ├─ Layout rules
      └─ Measurements
```

- 每个 `{Scenario}` 将同一组件在不同设备或可用空间下的状态与规则放在同一命名容器中。
- 用实际语义命名根 Frame、案例和内容容器；隐藏草稿、过期示例与无关节点不放在板块根 Frame 内。
