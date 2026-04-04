# Agent Tool

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `Agent` |
| 源码位置 | `src/tools/AgentTool/AgentTool.tsx` |
| 类型 | 子代理启动工具 |
| 并发安全 | 是 |

## 功能描述

启动新的子代理来自主处理复杂的多步骤任务。每种代理类型拥有特定的能力和可用工具集。

## 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `description` | string | 是 | 3-5 词任务简述 |
| `prompt` | string | 是 | 代理要执行的任务 |
| `subagent_type` | string | 否 | 子代理类型 |
| `model` | enum | 否 | 模型覆盖: `sonnet`, `opus`, `haiku` |
| `run_in_background` | boolean | 否 | 是否后台运行 |
| `name` | string | 否 | 代理名称 |
| `team_name` | string | 否 | 团队名称 |
| `mode` | string | 否 | 运行模式 |
| `isolation` | enum | 否 | 隔离模式: `worktree`, `remote` |
| `cwd` | string | 否 | 工作目录 |

## 输出结构

同步完成:
```json
{
  "status": "completed",
  "result": "..."
}
```

异步启动:
```json
{
  "status": "async_launched",
  "agentId": "...",
  "outputFile": "..."
}
```

## 内置代理类型

| 类型 | 说明 |
|------|------|
| `general-purpose` | 通用代理,研究、搜索、多步骤任务 |
| `Explore` | 快速代码库探索 |
| `Plan` | 软件架构规划 |
| `claude-code-guide` | Claude Code/API/SDK 使用指南 |
| `verification` | 验证测试专家 |
| `statusline-setup` | 状态栏配置 |

## 提示词要点

- 像简报同事一样描述任务,提供完整上下文
- 独立任务可并行启动多个代理
- 前台代理用于需要结果才能继续的场景
- 后台代理用于有独立并行工作的场景
- 可通过 SendMessage 继续已有代理
- `isolation: "worktree"` 在临时 git worktree 中隔离运行
- 超过 120 秒自动转入后台 (当启用时)

## 代理加载机制

除内置代理外,还支持从以下位置加载自定义代理:
- 用户级: `~/.claude/agents/*.md`
- 项目级: `.claude/agents/*.md`
- 托管级: 通过管理配置

自定义代理通过 Markdown frontmatter 定义配置:
```yaml
---
agentType: my-agent
tools: [Read, Write, Bash]
model: sonnet
---
系统提示词内容...
```
