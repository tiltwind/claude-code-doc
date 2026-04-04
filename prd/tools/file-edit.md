# File Edit Tool (Edit)

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `Edit` |
| 源码位置 | `src/tools/FileEditTool/FileEditTool.ts` |
| 类型 | 文件编辑工具 |
| 搜索提示 | "modify file contents in place" |

## 功能描述

在文件中执行精确的字符串替换。

## 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `file_path` | string | 是 | 文件的绝对路径 |
| `old_string` | string | 是 | 要替换的文本 |
| `new_string` | string | 是 | 替换后的文本 (必须与 old_string 不同) |
| `replace_all` | boolean | 否 | 替换所有出现 (默认 false) |

## 提示词要点

```
- 编辑前必须先用 Read 工具读取过文件
- 编辑 Read 输出的文本时,必须保留行号前缀之后的精确缩进
- 优先编辑已有文件,永远不要创建新文件除非绝对必要
- 不使用 emoji 除非用户明确请求
- 如果 old_string 在文件中不唯一,编辑会失败 — 提供更多上下文使其唯一,或使用 replace_all
- 使用 replace_all 可在文件中重命名变量等
```

## 关键常量

- `MAX_EDIT_FILE_SIZE`: 1 GiB

## 安全特性

- `old_string !== new_string` 验证
- 拒绝规则检查
- 文件大小限制
- 权限检查
- UNC 路径安全阻止
- 团队内存秘密守卫
