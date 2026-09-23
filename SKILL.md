---
name: case-ppt-simple
description: 病例汇报 PPT 保守改造。模板骨架（页面尺寸、主题、背景、Logo、母版、版式、页眉页脚、页码、页序、动画）零改动；但文字内容、文本格式（字体/字号/字重/颜色/对齐/行距/缩进/项目符号）以及既有表格与图片均可编辑，并可新增图片/表格/文本框/形状；病例内容可自行生成。交付按 NAME-科室-病例汇报 命名。与 case-report-ppt（重排版版）互为姊妹项目。
compatibility: opencode
metadata:
  format: agent-skills
  locale: zh-CN
---

# 病例汇报 PPT 保守改造（case-ppt-simple）

一句话：**模板骨架不动，内容与文字随意编辑。**

- **骨架（保守）**：不改页面尺寸/比例、主题、主题色、主背景、Logo、母版与版式、页眉页脚、页码体系、页序，也不增删动画与切换。
- **内容（自由）**：文字可任意改写、增删；文本格式可改（字体、字号、字重、颜色、对齐、行距、缩进、项目符号）；**既有表格与图片可编辑**；可新增图片、表格、文本框、形状。
- **病例内容可生成**：可自行编写去标识化教学病例，不受"不补造"限制。

## 约定常量

- 交付命名：`NAME-科室-病例汇报.pptx`（`NAME` 为汇报人，科室为轮转科室；不加病种/日期）。
- 工作/恢复副本：`NAME-科室-病例汇报_working.pptx`；交付后删除，只在目标目录留最终版与原件。
- 异常值强调色：`#EE0000`，只标关键数值，不整段染红。
- 新增对象的字体族与字号区间与全篇一致；正文不低于 16 pt。

## 准备（开始前）

0. **检查是否已安装 ppt-mcp（PowerPoint MCP）；未装则安装并让用户重启后继续**。
   - 项目与依赖：https://github.com/ykuwai/ppt-mcp ，以 `uvx ppt-mcp` 启动；需要 [uv](https://docs.astral.sh/uv/getting-started/installation/) 与本机 Microsoft PowerPoint（Windows / macOS）。
   - 探测：调用一次 `ppt_get_app_info` 或 `ppt_list_presentations`。**有返回 → 已安装，跳过安装，直接继续**。
   - **未安装 → 执行安装**：
     1. 确认 `uv` 可用（`uv --version`）；缺失则先安装 uv。
     2. 把 ppt-mcp 写入当前 MCP 客户端配置：
        - OpenCode（`opencode.json`）：`{"$schema":"https://opencode.ai/config.json","mcp":{"powerpoint":{"type":"local","command":["uvx","ppt-mcp"],"enabled":true}}}`
        - 其他客户端（Claude Desktop / Cursor / `.mcp.json`）：`{"mcpServers":{"powerpoint":{"command":"uvx","args":["ppt-mcp"]}}}`
     3. **提示用户重启 OpenCode / 会话**：MCP 只在启动时加载，重启后重新发起任务即可继续。
     4. 重启后仍不可用 → 退回 PowerShell COM 或 python-pptx；确无自动化能力时，只整理内容与可执行修改清单，不伪称已改 PPT。

## 开始

1. 先用文件读取能力加载 [references/workflow.md](references/workflow.md)；编辑前先创建并验证安全副本。
2. 盘点源 PPT：页数、页序、版式、母版、字体规格、文本框、图片、表格、图表、组合对象、动画与切换。
3. 按任务读取对应参考文件：

| 情形 | 必读文件 |
| --- | --- |
| 工作流、增页、溢出、交付 | [references/workflow.md](references/workflow.md) |
| 允许改什么、新增元素怎么放 | [references/layout-design.md](references/layout-design.md) |
| 内容生成、数值/日期一致、脱敏 | [references/medical-content.md](references/medical-content.md) |
| 工具操作与禁用清单 | [references/powerpoint-technical.md](references/powerpoint-technical.md) |
| 完成前逐项核对 | [references/validation.md](references/validation.md) |
| 允许/禁止的具体例子 | [references/examples.md](references/examples.md) |

## 铁律（不可突破）

**不动（骨架）**
- 不改页面尺寸/比例、主题、主题色、主背景、Logo、母版与版式、页眉页脚、页码体系。
- 不改页序；除授权外不增删页（增页必须复用与源页完全相同的版式）。
- 不增删动画与切换。
- 不移动、不缩放、不旋转、不删除、不组合/解组**表格与图片以外**的原有对象（如文本框、形状、图表、SmartArt、嵌入对象）。

**可改（内容与文字）**
- 文字内容：任意替换、增删。
- 文本格式：字体、字号、字重、颜色、对齐、行距、缩进、项目符号。
- 异常值用 `#EE0000` 标注（只标数值）。
- **既有表格**：改单元格内容与格式、增删行列、调整列宽行高与位置。
- **既有图片**：替换、裁剪、缩放、移动、调整亮度/对比度等。

**可增**
- 新增图片、表格、文本框、形状；放在不遮挡既有内容的位置。
- 新增对象的颜色、填充、边框、字体可自定，但只影响该新对象。

**脱敏（必要例外）**
- 源中含真实可识别信息时，遮蔽或删除标识信息，并在交付说明中记录。

## 默认决策

- `layout_change_level = 0`（骨架不动）；重排卡片/分栏请改用 `case-report-ppt`。
- 内容可生成：据科室与病种自行编写去标识化教学病例；同一份内数值、单位、日期、诊断名称保持一致。
- 溢出：可调整该文本框的字号（正文不低于 16 pt）或行距/缩进；仍放不下则新增一页（复制既有页保留版式）。
- 表格/图片优先"新增"承载新信息；也允许直接编辑既有表格与图片。
- 幻灯片（正文、页脚、备注、图片替代文本）不出现"教学病例模拟数据""模拟数据""仅用于演示"等字样。

## 交付

- 保存为 `NAME-科室-病例汇报.pptx`；交付后删除 `_working` 副本，只保留最终版与原件。
- 汇报时说明：输出路径、改动范围（文字/格式/表格/图片/新增元素/新增页）、是否有脱敏、以及"骨架（主题、母版、版式、页序、动画）零改动"的核对结论。
