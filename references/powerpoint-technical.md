# PowerPoint 技术注意（保守版）

原则：**只改文字内容，不碰任何格式；需要新东西就新增对象。**

## 就地改字（安全做法）

- 首选只改目标 run 的文本，保留其字符属性：
  - python-pptx：`run.text = "..."`，不新建 run、不动 `rPr`；
  - MCP：优先用"按范围/按搜索替换"的文本接口，且替换后核对被替换段的字体/字号/颜色未变。
- 避免"整体重写形状文本"的接口（如 `ppt_set_text` 只给 `text`、全量写入），它会重置字体、段落、项目符号与运行级颜色。
- 文案变长会改变原文本框的排版观感——若溢出或换行异常，按 [workflow.md](workflow.md) §6 处理（精简或增页），不要缩小字号。

## 明确禁用的操作

- 位置/尺寸类：`ppt_update_shape` 改 left/top/width/height/rotation；拖拽、对齐、分布、翻转、锁定比例。
- 样式类：`ppt_set_fill`、`ppt_set_line`、`ppt_set_shadow`、`ppt_set_glow`、`ppt_set_reflection`、`ppt_set_soft_edge`、`ppt_copy_formatting`、`ppt_format_text`（作用于既有形状）、`ppt_set_default_fonts`、`ppt_set_default_shape_style`、`ppt_apply_theme`、`ppt_set_theme_colors`、`ppt_set_slide_background`。
- 结构类：`ppt_delete_shape`、`ppt_group_shapes`、`ppt_ungroup_shapes`、`ppt_set_shape_zorder`（对原对象）、`ppt_copy_shape_to_slide`（除非新建页内使用）、`ppt_set_headers_footers`。
- 页序类：`ppt_move_slide`、`ppt_delete_slide`。

## 新增元素（允许）

- `ppt_add_table` / `ppt_add_picture` / `ppt_add_textbox` / `ppt_add_shape`（放在空白区）。
- 新增对象可以有自己的样式（填充、边框、字体），但仅限该新对象。
- 新增表格若用样式 API，只作用于新表，不要动既有表。
- 新增后核对：新对象未压住原文、未导致原对象位置变化。

## 逐页回读核对

- 每批改动后回读原对象的关键属性，与基线快照比对：
  - `left/top/width/height` 全等；
  - 字体名、字号、字重、颜色、对齐、缩进全等；
  - 页数、页序、母版、背景、页眉页脚未变。
- 视觉核对与结构核对都要做；渲染图只能验证视觉，不能证明格式未被改。

## 兼容性与恢复

- 警惕 SmartArt、嵌入对象、媒体、图表、公式、组合形状、母版占位符、动画触发器；这些一律不碰，需要信息就用新增对象另建。
- 另存 `.pptx` 后重新打开检查；出现"修复演示文稿"提示时，回到最近恢复点，不在损坏文件上继续改。
- 能力不足时，交付稳定版本并如实标注哪些新增未完成，不伪称已改。
