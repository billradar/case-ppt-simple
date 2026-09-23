# case-ppt-simple

病例汇报 PPT 的**保守改造**方案：在不惊动原模板的前提下，把内容补齐、把图表加上。

- **只增不改**：绝不改动现有 PPT 的排版与格式——不移动、不缩放、不删除、不改字体/字号/颜色、不动母版/版式/页序/动画。
- **允许新增**：可在空白区新增图片、表格、文本框等独立元素。
- **内容自由**：可自行生成、补全教学病例内容（病史、查体、检验、诊断、治疗、转归等），无需"不补造"约束。
- **命名规范**：交付文件统一命名为 `NAME-科室-病例汇报.pptx`。

## 适用场景

- 医院/科室有固定 PPT 模板，只允许填内容，不允许改设计。
- 只需把一段文字换清楚、把化验结果做成表格、补一张示意图。
- 希望在保留原模板观感的同时提高可读性与完整度。

## 与姊妹项目的关系

| 项目 | 定位 | 排版 |
| --- | --- | --- |
| [case-report-ppt](https://github.com/billradar/case-report-ppt) | 疾病内容重构、重排版 | 可适度重排（level 2） |
| **case-ppt-simple**（本项目） | 保守填内容、零改动 | 完全保持原 Layout（level 0） |

两者独立使用；本项目不并入 `case-report-ppt`。

## 使用

1. 用 OpenCode 加载 [SKILL.md](SKILL.md) 与其 `references/` 下的文件。
2. 编辑前先复制源 PPT 为工作副本，只改副本。
3. 只做两类改动：**就地改字** 与 **新增元素**；逐页按 [references/validation.md](references/validation.md) 核对"原对象几何与格式未变"。
4. 交付为 `NAME-科室-病例汇报.pptx`，交付后删除工作副本。

## 目录结构

```text
SKILL.md                      主入口：铁律、默认决策、交付
AGENTS.md                     给自动化代理的简明约束
references/
  workflow.md                 工作流：复制、盘点、改字、增页、交付
  layout-design.md            排版约束：允许新增什么、怎么放
  medical-content.md          内容生成、一致性与脱敏
  powerpoint-technical.md     工具操作：如何只改文字不改格式
  validation.md               完成前的逐项核对清单
  examples.md                 允许/禁止的具体例子
```

## 一句话记忆

**模板零改动，内容随便补；要新东西就新增，不改老的。**
