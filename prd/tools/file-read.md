# File Read Tool (Read)

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `Read` |
| 源码位置 | `src/tools/FileReadTool/FileReadTool.ts` |
| 类型 | 文件读取工具 |
| 只读 | 是 |

## 功能描述

从本地文件系统读取文件。支持文本文件、图片 (PNG, JPG 等)、PDF 文件和 Jupyter notebook。

## 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `file_path` | string | 是 | 文件的绝对路径 |
| `offset` | number | 否 | 起始行号 |
| `limit` | number | 否 | 读取行数 (默认 2000) |
| `pages` | string | 否 | PDF 页码范围 (如 "1-5") |

## 提示词要点

```
- 默认从文件开头读取最多 2000 行
- 已知需要文件哪部分时,只读取该部分 (对大文件很重要)
- 结果使用 cat -n 格式,行号从 1 开始
- 可读取图片文件 (多模态 LLM)
- 可读取 PDF 文件,大 PDF (超过 10 页) 必须提供 pages 参数,每次最多 20 页
- 可读取 Jupyter notebook,返回所有单元格及其输出
- 只能读文件不能读目录,读目录请用 ls 命令
```

## 关键常量

- `MAX_LINES_TO_READ`: 2000
- `FILE_UNCHANGED_STUB`: 文件未修改时的占位符

## 特殊处理

- 阻止读取设备路径 (`/dev/zero`, `/dev/random` 等)
- macOS 截图路径解析 (处理 thin-space/regular-space AM/PM 差异)
- 文件读取监听器系统 (`registerFileReadListener`)
- `MaxFileReadTokenExceededError` 异常处理
