---
name: vt-token-naming
description: 命名或评审 Reference、System、Component token；当任务读取或创建 Figma Variables 时，同时处理公开名与 slash path 的双向转换。
---

# VT Token Naming

使用“最短充分名称”：构成区分 token 含义所必需的词。先读取当前项目的 token 层级、词表和 Variables 组织；本技能的公式与词根不替代项目已确认的命名契约。

## 本机项目数据

`@data/links.csv` 是可选的本机链接表，不进入 Git。

1. 任务需要已确认的 token 文件或项目文档链接时，先读取该文件。
2. 没有对应记录时，使用本次任务提供的链接，不猜测链接。
3. 用户提供或确认稳定链接后，创建 `data/`（如不存在）并以 `key,url` 格式新增或更新记录。
4. 只保存项目导航链接，不保存凭证、导出内容或敏感 token 数据。

## 执行

1. 判断层级：
   - Reference：品牌基础值、刻度或程度。
   - System：跨组件复用的用途、程度或场景语义。
   - Component：单个组件内的设计职责。

   信息不足以唯一判断时，只询问缺少的语义。

2. 只读取当前层级：
   - Reference：[references/reference-tokens.md](references/reference-tokens.md)
   - System：[references/system-tokens.md](references/system-tokens.md)
   - Component：[references/component-tokens.md](references/component-tokens.md)

3. 按公式和词表构词。每个可选词根都必须改变含义；词表缺项时，提出最小候选词并等待确认。

4. 默认只给一个推荐名。只有存在真实语义歧义时，最多给两个备选并说明差异。

5. 仅当任务要求读取或创建 Figma Variables 时，再读取 [references/figma-variable-paths.md](references/figma-variable-paths.md)，同时处理公开名、slash path 与 Variable 完整性门禁。

完成标准：名称符合对应层公式、大小写和项目词表，且每个保留词都用于区分含义；Figma 分支还必须通过公开名/slash path 双向回环及 Variable 完整性门禁。
