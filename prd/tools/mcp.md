# MCP Tool

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | 动态 (由 MCP 服务器定义) |
| 源码位置 | `src/tools/MCPTool/MCPTool.ts` |
| 类型 | MCP 协议工具骨架 |

## 功能描述

通用 MCP 工具骨架。实际的工具名称、描述和调用方法在 `mcpClient.ts` 运行时为每个 MCP 工具覆盖。

## 相关工具

| 工具 | 源码 | 说明 |
|------|------|------|
| `ListMcpResources` | `src/tools/ListMcpResourcesTool/` | 列出 MCP 服务器可用资源 |
| `ReadMcpResource` | `src/tools/ReadMcpResourceTool/` | 读取 MCP 服务器特定资源 |
| `McpAuth` | - | MCP 认证 |

## 设计要点

- 输入 schema 为 `z.object({}).passthrough()`,允许任意参数透传
- 实际功能完全由 MCP 服务器定义
- 作为 MCP 协议的桥梁工具
