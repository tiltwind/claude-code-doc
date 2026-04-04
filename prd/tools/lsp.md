# LSP Tool

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `LSP` |
| 源码位置 | `src/tools/LSPTool/LSPTool.ts` |
| 类型 | 语言服务器协议工具 |
| 启用条件 | `ENABLE_LSP_TOOL` 环境变量 |

## 功能描述

与 Language Server Protocol (LSP) 服务器交互,获取代码智能功能。

## 支持的操作

| 操作 | 说明 |
|------|------|
| `goToDefinition` | 跳转到定义 |
| `findReferences` | 查找引用 |
| `hover` | 悬停信息 |
| `documentSymbol` | 文档符号 |
| `workspaceSymbol` | 工作区符号 |
| `goToImplementation` | 跳转到实现 |
| `prepareCallHierarchy` | 准备调用层次 |
| `incomingCalls` | 传入调用 |
| `outgoingCalls` | 传出调用 |

## 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `operation` | enum | 是 | 上述操作之一 |
| `filePath` | string | 是 | 文件路径 |
| `line` | number | 是 | 行号 (1-based) |
| `character` | number | 是 | 列号 (1-based) |
