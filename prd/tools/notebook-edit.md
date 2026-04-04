# Notebook Edit Tool

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `NotebookEdit` |
| 源码位置 | `src/tools/NotebookEditTool/NotebookEditTool.ts` |
| 类型 | Jupyter notebook 编辑工具 |

## 功能描述

替换 Jupyter notebook (.ipynb) 中特定单元格的内容。

## 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `notebook_path` | string | 是 | notebook 的绝对路径 |
| `cell_id` | number | 是 | 单元格编号 (0-indexed) |
| `new_source` | string | 是 | 新的单元格内容 |
| `cell_type` | string | 否 | 单元格类型 |
| `edit_mode` | enum | 否 | `replace` / `insert` / `delete` |

## 提示词要点

```
- 完全替换 Jupyter notebook 中特定单元格的内容
- notebook_path 必须是绝对路径
- cell_number 是 0-indexed
- edit_mode=insert: 在指定位置插入新单元格
- edit_mode=delete: 删除指定位置的单元格
```
