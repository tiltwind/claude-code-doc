# Skill Tool

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `Skill` |
| 源码位置 | `src/tools/SkillTool/SkillTool.ts` |
| 类型 | 技能执行工具 |

## 功能描述

在主对话中执行技能 (slash commands)。当用户引用 "/<skill-name>" 时调用。

## 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `skill` | string | 是 | 技能名称 |
| `args` | string | 否 | 技能参数 |

## 提示词要点

```
- 用户引用 "slash command" 或 "/<something>" 时使用此工具
- 匹配用户请求的技能时,必须在生成其他响应前调用
- 不要提及技能而不实际调用此工具
- 不要在已运行的技能上再次调用
- 不用于内置 CLI 命令 (如 /help, /clear)
```

## 实现细节

- 通过 `getAllCommands()` 聚合本地命令和 MCP 技能
- `executeForkedSkill()` 在隔离的子代理上下文中运行技能,带 token 预算
- 支持内置命令和市场插件命令
- 实验性技能搜索功能 (EXPERIMENTAL_SKILL_SEARCH feature flag)
- 技能列表描述的预算管理 (上下文窗口的 1%)
