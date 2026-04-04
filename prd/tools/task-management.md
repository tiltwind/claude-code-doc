# Task Management Tools

## 概述

任务管理工具集,用于创建、跟踪和管理编码会话中的结构化任务。

## TaskCreate

| 属性 | 值 |
|------|------|
| 工具名称 | `TaskCreate` |
| 源码位置 | `src/tools/TaskCreateTool/TaskCreateTool.ts` |
| 启用条件 | `isTodoV2Enabled()` |

### 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `subject` | string | 是 | 任务标题 |
| `description` | string | 否 | 详细描述和上下文 |
| `activeForm` | string | 否 | 当前进行时描述 |
| `metadata` | object | 否 | 元数据 |

### 使用场景
- 复杂多步骤任务
- 计划模式
- 用户提供多个任务时
- 跳过: 单一简单任务

---

## TaskGet

| 属性 | 值 |
|------|------|
| 工具名称 | `TaskGet` |
| 源码位置 | `src/tools/TaskGetTool/TaskGetTool.ts` |

### 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `taskId` | string | 是 | 任务 ID |

### 输出
返回完整任务详情: id, subject, description, status, blocks, blockedBy

---

## TaskUpdate

| 属性 | 值 |
|------|------|
| 工具名称 | `TaskUpdate` |
| 源码位置 | `src/tools/TaskUpdateTool/TaskUpdateTool.ts` |

### 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `taskId` | string | 是 | 任务 ID |
| `subject` | string | 否 | 更新标题 |
| `description` | string | 否 | 更新描述 |
| `activeForm` | string | 否 | 当前进行时描述 |
| `status` | enum | 否 | `pending` / `in_progress` / `completed` / `deleted` |
| `addBlocks` | string[] | 否 | 添加阻塞的任务 |
| `addBlockedBy` | string[] | 否 | 添加被阻塞的任务 |
| `owner` | string | 否 | 负责人 |
| `metadata` | object | 否 | 元数据 |

### 状态流转
`pending` → `in_progress` → `completed` (或 `deleted`)

---

## TaskList

| 属性 | 值 |
|------|------|
| 工具名称 | `TaskList` |
| 源码位置 | `src/tools/TaskListTool/TaskListTool.ts` |

无输入参数。返回所有任务的摘要: id, subject, status, owner, blockedBy。

### 团队工作流
启用代理集群时:
- 完成任务后检查 TaskList
- 优先处理最低 ID 的任务
- 认领未分配且未阻塞的任务

---

## TaskStop

| 属性 | 值 |
|------|------|
| 工具名称 | `TaskStop` |
| 源码位置 | `src/tools/TaskStopTool/TaskStopTool.ts` |
| 别名 | `KillShell` |

### 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `task_id` | string | 是 | 要停止的任务 ID |

---

## TaskOutput

| 属性 | 值 |
|------|------|
| 工具名称 | `TaskOutput` |
| 源码位置 | `src/tools/TaskOutputTool/TaskOutputTool.tsx` |
| 别名 | `AgentOutputTool`, `BashOutputTool` |
| 状态 | 已弃用,建议使用 Read 工具读取任务输出文件 |

### 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `task_id` | string | 是 | 任务 ID |
| `block` | boolean | 否 | 等待任务完成 |
| `timeout` | number | 否 | 超时时间 |
