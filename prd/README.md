# Claude Code — Product Requirements Document

> Anthropic 官方 CLI for Claude AI 的完整产品设计文档

## 文档总览

| 类别 | 文件数 | 说明 |
|------|-------|------|
| [产品概述](#产品概述) | 1 | 产品定位、目标用户、核心价值、设计原则、产品边界 |
| [架构设计](#架构设计) | 2 | 业务模块划分、平台入口、模块关系图、角色权限矩阵 |
| [数据字典](#数据字典) | 15 | 业务枚举和常量定义 |
| [数据模型](#数据模型) | 16 | 核心实体的属性、状态、状态流转、关联关系 |
| [业务流程](#业务流程) | 11 | 关键流程的步骤、规则、异常处理、流程图 |
| [应用与页面](#应用与页面) | 15 | CLI 应用框架、5 个页面设计 |
| [变更记录](#变更记录) | 4 | 需求变更追溯 |

---

## 产品概述

- [overview.md](overview.md) — 产品定位、目标用户、核心价值、7 大设计原则、产品边界

## 架构设计

- [architecture/architecture.md](architecture/architecture.md) — 平台入口设计、6 大业务模块组（Core / Extension / Configuration / DevOps / IO / Observability）、模块关系 Mermaid 图
- [architecture/roles.md](architecture/roles.md) — 8 个系统角色定义（End User / Claude Assistant / Sub-Agent / Coordinator / Teammate / Enterprise Admin / MCP Server / Hook Executor）、权限矩阵

## 数据字典

> 索引文件: [dictionaries/dictionaries.md](dictionaries/dictionaries.md)

### Core

| 字典 | 说明 |
|------|------|
| [dictionary-permission-mode.md](dictionaries/core/dictionary-permission-mode.md) | 权限模式: Default / Plan / AcceptEdits / Auto / Bypass |
| [dictionary-permission-behavior.md](dictionaries/core/dictionary-permission-behavior.md) | 权限行为: Allow / Deny / Ask |
| [dictionary-permission-decision.md](dictionaries/core/dictionary-permission-decision.md) | 权限决策结果: Approved / Denied / Ask-User |
| [dictionary-message-role.md](dictionaries/core/dictionary-message-role.md) | 消息角色: User / Assistant / System / Attachment / Progress |
| [dictionary-session-state.md](dictionaries/core/dictionary-session-state.md) | 会话状态: Initializing → Active → Processing → Compacting → Backgrounded → Ended |
| [dictionary-task-status.md](dictionaries/core/dictionary-task-status.md) | 任务状态: Pending → In Progress → Completed / Deleted |
| [dictionary-model-tier.md](dictionaries/core/dictionary-model-tier.md) | 模型层级: Opus / Sonnet / Haiku (含 1M 变体) |
| [dictionary-skill-execution-mode.md](dictionaries/core/dictionary-skill-execution-mode.md) | Skill 执行模式: Inline / Forked / Local / Local-JSX |
| [dictionary-hook-event-type.md](dictionaries/core/dictionary-hook-event-type.md) | Hook 事件: PreToolUse / PostToolUse / PostToolUseFailure / UserPromptSubmit / FileChanged / SubagentStart / Stop |
| [dictionary-settings-source.md](dictionaries/core/dictionary-settings-source.md) | 设置来源优先级: User < Project < Local < Flag < Policy |
| [dictionary-voice-state.md](dictionaries/core/dictionary-voice-state.md) | 语音状态: Idle / Recording / Processing |
| [dictionary-agent-type.md](dictionaries/core/dictionary-agent-type.md) | 6 种内建 Agent 类型: General Purpose / Explore / Plan / Verification / Claude Code Guide / Statusline Setup |
| [dictionary-task-type.md](dictionaries/core/dictionary-task-type.md) | 5 种任务类型: DreamTask / LocalAgentTask / RemoteAgentTask / InProcessTeammateTask / LocalShellTask |

### Tool System

| 字典 | 说明 |
|------|------|
| [dictionary-tool-category.md](dictionaries/tool-system/dictionary-tool-category.md) | 工具分类: File IO / Search / Shell / Web / Agent / Task / Planning / MCP / Scheduling / Code Intelligence / Worktree / Config / User Interaction |

## 数据模型

> 索引文件: [models/models.md](models/models.md)

### Core

> 索引文件: [models/core/models-core.md](models/core/models-core.md)

| 模型 | 模块 | 说明 |
|------|------|------|
| [model-session.md](models/core/conversation/model-session.md) | Conversation | 会话: 6 个状态、12 个状态流转 |
| [model-message.md](models/core/conversation/model-message.md) | Conversation | 消息: 5 种内容块类型 (text / tool_use / tool_result / image / file) |
| [model-conversation-history.md](models/core/conversation/model-conversation-history.md) | Conversation | 会话历史持久化记录 |
| [model-tool.md](models/core/tool-system/model-tool.md) | Tool System | 工具: fail-closed 安全默认、14 步流水线方法、isDestructive / isConcurrencySafe |
| [model-tool-result.md](models/core/tool-system/model-tool-result.md) | Tool System | 工具执行结果 |
| [model-task.md](models/core/task-management/model-task.md) | Task Management | 任务: 4 个状态、依赖关系跟踪 |
| [model-system-prompt.md](models/core/prompt-system/model-system-prompt.md) | Prompt System | 系统提示词: 静态/动态分区、Prompt Cache 边界标记 |

### Extension

> 索引文件: [models/extension/models-extension.md](models/extension/models-extension.md)

| 模型 | 模块 | 说明 |
|------|------|------|
| [model-skill.md](models/extension/skill/model-skill.md) | Skill | 自定义斜杠命令 |
| [model-plugin.md](models/extension/plugin/model-plugin.md) | Plugin | 第三方扩展: hooks / MCP / output styles / model hints / blocklist |
| [model-mcp-server.md](models/extension/mcp/model-mcp-server.md) | MCP | MCP 服务器: 4 个连接状态 |
| [model-mcp-resource.md](models/extension/mcp/model-mcp-resource.md) | MCP | MCP 资源 |

### Configuration

> 索引文件: [models/configuration/models-configuration.md](models/configuration/models-configuration.md)

| 模型 | 模块 | 说明 |
|------|------|------|
| [model-settings.md](models/configuration/settings/model-settings.md) | Settings | 多层设置合并 (user < project < local < flag < policy) |
| [model-permission-rule.md](models/configuration/permission/model-permission-rule.md) | Permission | 权限规则 (tool + behavior + source) |
| [model-hook.md](models/configuration/permission/model-hook.md) | Permission | Hook 自动化 (事件驱动的确定性 shell 命令) |

## 业务流程

> 索引文件: [procedures/procedures.md](procedures/procedures.md)

### Core

> 索引文件: [procedures/core/procedures-core.md](procedures/core/procedures-core.md)

| 流程 | 模块 | 说明 |
|------|------|------|
| [procedure-conversation-flow.md](procedures/core/conversation/procedure-conversation-flow.md) | Conversation | 对话主循环: 提交 → Hook → 上下文组装 → API 调用 → 工具执行循环 → 渲染 |
| [procedure-message-compaction.md](procedures/core/conversation/procedure-message-compaction.md) | Conversation | 四道压缩 (Snip → Micro → Context Collapse → Auto Compact) + Reactive Compact + Token Budget |
| [procedure-tool-execution.md](procedures/core/tool-system/procedure-tool-execution.md) | Tool System | **14 步执行流水线**: 查找 → 校验 → 分类器 → Hook → 权限 → 执行 → 追踪 → 结果 |
| [procedure-permission-check.md](procedures/core/permission/procedure-permission-check.md) | Permission | 三层防护网: Speculative Classifier + Hook Policy + Permission Decision |
| [procedure-prompt-assembly.md](procedures/core/prompt-system/procedure-prompt-assembly.md) | Prompt System | 系统提示词组装: 静态段 → 动态边界 → 动态段 → 工具提示 → Token 预算 |

### Extension

> 索引文件: [procedures/extension/procedures-extension.md](procedures/extension/procedures-extension.md)

| 流程 | 模块 | 说明 |
|------|------|------|
| [procedure-agent-spawning.md](procedures/extension/agent/procedure-agent-spawning.md) | Agent | 子 Agent 生成: fork-path cache 优化、worktree 隔离、生命周期管理 |
| [procedure-coordinator-workflow.md](procedures/extension/agent/procedure-coordinator-workflow.md) | Agent | 协调者模式: 任务分解 → 并行 Worker → 结果聚合 |

### DevOps

> 索引文件: [procedures/devops/procedures-devops.md](procedures/devops/procedures-devops.md)

| 流程 | 模块 | 说明 |
|------|------|------|
| [procedure-commit-flow.md](procedures/devops/git-workflow/procedure-commit-flow.md) | Git Workflow | AI 辅助 Git 提交 |
| [procedure-pr-creation.md](procedures/devops/git-workflow/procedure-pr-creation.md) | Git Workflow | 端到端 Commit → Push → PR 工作流 |

## 应用与页面

### CLI Terminal Application

> 框架文件: [applications/cli/application-cli.md](applications/cli/application-cli.md)
>
> 页面索引: [applications/cli/pages/pages-cli.md](applications/cli/pages/pages-cli.md)

| 页面 | 模块 | 说明 |
|------|------|------|
| [001-conversation-main.md](applications/cli/pages/core/conversation/001-conversation-main.md) | Conversation | 主对话界面: 状态栏 + 消息历史 + 输入区 + 工具栏 |
| [002-permission-dialog.md](applications/cli/pages/core/conversation/002-permission-dialog.md) | Permission | 权限弹窗: Allow / Allow Always / Deny / Deny Always |
| [001-task-list.md](applications/cli/pages/core/task-management/001-task-list.md) | Task Management | 任务列表: 状态指示器 + 进度条 + 依赖关系 |
| [001-settings-view.md](applications/cli/pages/configuration/settings/001-settings-view.md) | Settings | 设置管理: 多源合并显示 |
| [001-commit-view.md](applications/cli/pages/devops/git-workflow/001-commit-view.md) | Git Workflow | Git 提交界面: 变更分析 + 消息生成 + 执行状态 |

## 变更记录

| 日期 | 编号 | 名称 | 说明 |
|------|------|------|------|
| 2026-04-02 | [001](../changes/2026/04/02/2026-04-02-001-initial-prd/) | Initial PRD | 首次 PRD 文档生成 (59 文件) |
| 2026-04-02 | [002](../changes/2026/04/02/2026-04-02-002-prd-supplement/) | PRD Supplement | 基于架构深度解析 PDF 补充: 平台入口、Prompt 系统、14 步流水线、四道压缩、三层防护网、Agent 类型、设计原则 |
