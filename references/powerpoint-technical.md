# PowerPoint 技术注意（保守版）

原则：**骨架（主题/母版/版式/背景/页序/动画）不动，非表格图片原对象的几何不动；文字、格式、表格、图片可改。**

## 允许的操作

- 文字与文本格式：
  - 改 run 文本；改字体、字号、字重、颜色、对齐、行距、缩进、项目符号。
  - MCP：`ppt_set_text`（限文本框）、`ppt_format_text` / `ppt_format_text_range`（限文本格式）、`ppt_set_bullet`、`ppt_set_paragraph_format`。
- 既有表格：`ppt_set_table_data` / `ppt_set_table_cell` / `ppt_add_table_row` / `ppt_add_table_column` / `ppt_delete_table_row` / `ppt_delete_table_column` / `ppt_set_table_layout` / `ppt_set_table_borders` / `ppt_set_table_style` / `ppt_move`（表格对象）。
- 既有图片：`ppt_update_image` / `ppt_crop_picture` / `ppt_set_image_size` / `ppt_set_picture_format` / `ppt_update_shape`（仅图片对象的位置尺寸）。
- 新增元素：`ppt_add_table` / `ppt_add_picture` / `ppt_add_textbox` / `ppt_add_shape`（空白区）。
- 增页：复制既有页（`ppt_duplicate_slide` 或复制相同版式的新页）。

## 明确禁用

- 主题/母版/版式/背景：`ppt_apply_theme`、`ppt_set_theme_colors`、`ppt_set_default_fonts`、`ppt_set_default_shape_style`、`ppt_set_slide_background`、`ppt_set_headers_footers`。
- 页序：`ppt_move_slide`；`ppt_delete_slide`（除授权）。
- 动画：`ppt_add_animation` / `ppt_update_animation` / `ppt_remove_animation` / `ppt_clear_animations` / `ppt_set_slide_transition`。
- 对**非表格/图片对象**改几何或删除：`ppt_update_shape` 改 left/top/width/height/rotation、`ppt_delete_shape`、`ppt_group_shapes`、`ppt_ungroup_shapes`、`ppt_align_shapes`、`ppt_distribute_shapes`、`ppt_flip_shape`、`ppt_set_shape_zorder`、`ppt_copy_shape_to_slide`。

## 就地改字的稳妥做法

- 优先只改目标 run 的文本：
  - python-pptx：`run.text = "..."`，需要时再单独设该 run 的字体/字号/颜色；
  - MCP：用 `ppt_set_text` 的 `search_text` 或 `start/length` 局部替换，避免整形状全量重置。
- 若一个字体内中英混排，注意同时设置西文字体与东亚字体，避免回退。
- 改字号后核对是否溢出文本框；溢出按 [workflow.md](workflow.md) §6 处理。

## 逐页回读核对

- 每批改动后核对：
  - 骨架未变：页面尺寸、主题、母版、版式、背景、页眉页脚、页码、页序、动画；
  - 非表格/图片原对象的 `left/top/width/height` 与基线全等；
  - 目标文字与格式确实按要求更新。
- 视觉核对与结构核对都要做；渲染图只能验证视觉。

## 兼容性与恢复

- 警惕 SmartArt、嵌入对象、媒体、图表、公式、组合形状、母版占位符、动画触发器；这些一律不动其几何，需要信息就用新增对象另建。
- 另存 `.pptx` 后重新打开检查；出现"修复演示文稿"提示时，回到最近恢复点。
- 能力不足时，交付稳定版本并如实标注未完成项，不伪称已改。
