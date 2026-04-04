# Team Management Tools

## 概述

团队管理工具,用于创建和删除多代理协作团队。

## TeamCreate

| 属性 | 值 |
|------|------|
| 工具名称 | `TeamCreate` |
| 源码位置 | `src/tools/TeamCreateTool/TeamCreateTool.ts` |
| 启用条件 | `isAgentSwarmsEnabled()` |

### 功能描述

创建团队用于协调多个代理。创建团队配置和任务目录:
- 配置: `~/.claude/teams/{team-name}/config.json`
- 任务: `~/.claude/tasks/{team-name}/`

### 团队工作流

1. 创建团队
2. 创建任务 (TaskCreate)
3. 生成队友 (Agent with team_name)
4. 分配任务
5. 处理空闲状态
6. 发现团队成员
7. 任务列表协调

---

## TeamDelete

| 属性 | 值 |
|------|------|
| 工具名称 | `TeamDelete` |
| 源码位置 | `src/tools/TeamDeleteTool/TeamDeleteTool.ts` |

### 功能描述

移除团队和任务目录。如果团队仍有活跃成员会失败 — 必须先终止队友。
