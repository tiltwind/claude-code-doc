# Statusline Setup Agent

## 基本信息

| 属性 | 值 |
|------|------|
| Agent Type | `statusline-setup` |
| 源码位置 | `src/tools/AgentTool/built-in/statuslineSetup.ts` |
| 模型 | 默认子代理模型 |
| 可用工具 | `Read`, `Edit` |
| 来源 | built-in |

## 使用场景

用于配置用户的 Claude Code 状态栏设置。

## 设计要点

- 工具集极为精简,仅包含 `Read` 和 `Edit`
- 专注于读取和修改配置文件
- 单一职责: 状态栏配置
