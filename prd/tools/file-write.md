# File Write Tool (Write)

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `Write` |
| 源码位置 | `src/tools/FileWriteTool/FileWriteTool.ts` |
| 类型 | 文件写入工具 |

## 功能描述

将文件写入本地文件系统。如果目标路径已存在文件则覆盖。

## 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `file_path` | string | 是 | 文件的绝对路径 |
| `content` | string | 是 | 要写入的内容 |

## 输出结构

```json
{
  "type": "create | update",
  "filePath": "...",
  "content": "...",
  "structuredPatch": "...",
  "originalFile": "...",
  "gitDiff": "..."
}
```

## 提示词要点

```
- 如果是已有文件,必须先用 Read 工具读取内容
- 优先使用 Edit 工具修改已有文件 — Edit 只发送差异
- 只在创建新文件或完全重写时使用此工具
- 永远不创建文档文件 (*.md) 或 README 文件,除非用户明确请求
- 不使用 emoji 除非用户明确请求
```

## 安全特性

- 写权限检查
- UNC 路径安全阻止
- 文件修改时间验证
- 团队内存秘密检查
