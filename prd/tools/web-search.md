# Web Search Tool

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `WebSearch` |
| 源码位置 | `src/tools/WebSearchTool/WebSearchTool.ts` |
| 类型 | 网络搜索工具 |
| 延迟加载 | 是 |
| 并发安全 | 是 |

## 功能描述

执行网络搜索查询,返回搜索结果。基于 Anthropic SDK 的 `BetaWebSearchTool20250305`。

## 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `query` | string | 是 | 搜索查询 (最少 2 字符) |
| `allowed_domains` | string[] | 否 | 允许的域名列表 |
| `blocked_domains` | string[] | 否 | 阻止的域名列表 |

## 输出结构

```json
{
  "query": "...",
  "results": [...],
  "durationSeconds": 1.5
}
```

## 提示词要点

```
- 执行网络搜索并返回结果
- 响应中必须包含 Sources 部分
- 动态注入当前月份/年份以优化搜索
- 支持域名过滤
- 仅限美国可用
```

## 可用性

- firstParty: 可用
- Vertex: Claude 4.0+ 可用
- Foundry: 可用
- 最大使用次数: 8 次/查询
