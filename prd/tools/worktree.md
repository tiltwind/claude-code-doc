# Worktree Tools

## 概述

Git worktree 隔离工具,用于在临时 git worktree 中进行隔离开发。

## EnterWorktree

| 属性 | 值 |
|------|------|
| 工具名称 | `EnterWorktree` |
| 源码位置 | `src/tools/EnterWorktreeTool/EnterWorktreeTool.ts` |
| 启用条件 | `isWorktreeModeEnabled()` |

### 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `name` | string | 否 | worktree 名称 (slug) |

### 输出
返回 worktree path, branch, message。

### 提示词要点

```
- 创建隔离的 git worktree 并切换会话
- 仅在用户明确说 "worktree" 时使用
- 在 .claude/worktrees/ 内创建 worktree
- 非 git 仓库中委托给 hooks
```

---

## ExitWorktree

| 属性 | 值 |
|------|------|
| 工具名称 | `ExitWorktree` |
| 源码位置 | `src/tools/ExitWorktreeTool/ExitWorktreeTool.ts` |

### 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `action` | enum | 是 | `keep` 或 `remove` |
| `discard_changes` | boolean | 否 | 是否丢弃更改 |

### 安全特性
- 未提交更改的安全检查
- 仅操作当前会话创建的 worktree
- 恢复工作目录并清除 CWD 相关缓存
