# Plan Mode Tools

## 概述

计划模式工具,用于进入/退出只读规划模式。

## EnterPlanMode

| 属性 | 值 |
|------|------|
| 工具名称 | `EnterPlanMode` |
| 源码位置 | `src/tools/EnterPlanModeTool/EnterPlanModeTool.ts` |

### 功能描述

进入计划模式。将权限模式设为 `plan`,限制为只读探索。无输入参数。

### 使用场景

**应使用计划模式的场景**:
- 架构歧义
- 需求不清晰
- 多文件变更

**应跳过计划模式的场景**:
- 简单任务
- 指令明确

### 提示词变体
- **外部用户版**: 更积极地推荐使用计划模式
- **内部用户版**: 更保守,倾向于直接开始工作并提出具体问题

---

## ExitPlanMode

| 属性 | 值 |
|------|------|
| 工具名称 | `ExitPlanMode` |
| 源码位置 | `src/tools/ExitPlanModeTool/ExitPlanModeV2Tool.ts` |

### 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `allowedPrompts` | array | 否 | 基于提示的实现权限 |

### 输出
包含 plan content、file path、approval status。

### 提示词要点

```
- 仅在计划模式下且计划文件准备好时使用
- 工具从已写入的文件中读取计划 (不通过参数传递)
- 仅用于实现规划,不用于研究任务
- 不要与 AskUserQuestion 混淆用于计划审批
```
