# Bash Tool

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `Bash` |
| 源码位置 | `src/tools/BashTool/BashTool.tsx` |
| 类型 | 命令执行工具 |
| 大小 | ~160KB (含安全检查) |

## 功能描述

执行给定的 bash 命令并返回输出。工作目录在命令间持久化,但 shell 状态不持久化。

## 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `command` | string | 是 | 要执行的命令 |
| `description` | string | 否 | 命令简述 |
| `timeout` | number | 否 | 超时毫秒数 (最大 600000) |
| `run_in_background` | boolean | 否 | 后台运行 |

## 关键组件

| 文件 | 用途 |
|------|------|
| `BashTool.tsx` | 主实现 (~160KB) |
| `UI.tsx` | 结果渲染 (~25KB) |
| `bashSecurity.ts` | 安全验证 (~102KB) |
| `bashPermissions.ts` | 权限规则 (~98KB) |
| `pathValidation.ts` | 路径验证 (~43KB) |
| `prompt.ts` | 提示词定义 |

## 提示词要点

### 工具优先级

应优先使用专用工具而非 Bash:
- 文件搜索: 用 `Glob` 而非 `find`/`ls`
- 内容搜索: 用 `Grep` 而非 `grep`/`rg`
- 读文件: 用 `Read` 而非 `cat`/`head`/`tail`
- 编辑文件: 用 `Edit` 而非 `sed`/`awk`
- 写文件: 用 `Write` 而非 `echo`/`cat`
- 通信: 直接输出文本而非 `echo`/`printf`

### Git 安全协议

- 永远不修改 git config
- 永远不运行破坏性 git 命令除非用户明确要求
- 永远不跳过 hooks (--no-verify)
- 永远不 force push 到 main/master
- 始终创建新提交而非 amend
- 提交消息通过 HEREDOC 传递以确保格式正确

### Git 提交流程

1. 并行运行 `git status` + `git diff` + `git log`
2. 分析变更,起草提交消息
3. 并行运行 `git add` + `git commit` + `git status`
4. 提交消息末尾附加 `Co-Authored-By: Claude <noreply@anthropic.com>`

### PR 创建流程

1. 并行运行 `git status` + `git diff` + `git log` + `git diff base...HEAD`
2. 分析所有提交,起草 PR 标题和摘要
3. 并行创建分支/推送/创建 PR

### 命令分类

系统内部将命令分为以下类别:
- `BASH_SEARCH_COMMANDS`: 搜索类命令
- `BASH_READ_COMMANDS`: 读取类命令
- `BASH_LIST_COMMANDS`: 列表类命令
- `BASH_SEMANTIC_NEUTRAL_COMMANDS`: 语义中性命令
- `BASH_SILENT_COMMANDS`: 静默命令

## 安全特性

- 沙箱模式: 可配置的命令沙箱限制
- 路径验证: 阻止 UNC 路径等不安全路径
- 命令安全分类: 自动识别高风险命令
- 权限检查: 根据命令类型匹配权限规则
