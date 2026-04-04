# Remote Trigger Tool

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `RemoteTrigger` |
| 源码位置 | `src/tools/RemoteTriggerTool/RemoteTriggerTool.ts` |
| 启用条件 | `AGENT_TRIGGERS_REMOTE` feature flag |
| 认证 | 需要 OAuth |

## 功能描述

通过 claude.ai CCR API 管理计划的远程 Claude Code 代理 (triggers)。

## 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `action` | enum | 是 | `list` / `get` / `create` / `update` / `run` |
| `trigger_id` | string | 否 | trigger ID |
| `body` | object | 否 | 请求体 |
