# Tool Search Tool

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `ToolSearch` |
| 源码位置 | `src/tools/ToolSearchTool/ToolSearchTool.ts` |
| 启用条件 | `isToolSearchEnabledOptimistic()` |

## 功能描述

获取延迟加载工具的完整 schema 定义,使其可被调用。延迟工具在 system-reminder 消息中仅以名称出现,在获取 schema 前无法调用。

## 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `query` | string | 是 | 搜索查询 |
| `max_results` | number | 否 | 最大结果数 (默认 5) |

### 查询格式

| 格式 | 说明 | 示例 |
|------|------|------|
| `select:<name>` | 精确选择工具 | `select:Read,Edit,Grep` |
| keywords | 关键词搜索 | `notebook jupyter` |
| `+prefix search` | 名称前缀 + 关键词 | `+slack send` |

## 输出格式

返回匹配工具的完整 JSONSchema 定义,封装在 `<functions>` 块中。

## 不应延迟的工具

以下工具永远不会被延迟加载:
- ToolSearch 自身
- Agent (当 fork 启用时)
- Brief
- SendUserFile
