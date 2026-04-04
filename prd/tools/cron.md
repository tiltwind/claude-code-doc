# Cron (Scheduled Task) Tools

## 概述

定时任务工具集,用于创建、管理和列出定时任务。

| 启用条件 | `AGENT_TRIGGERS` feature flag |

## CronCreate

| 属性 | 值 |
|------|------|
| 工具名称 | `CronCreate` |
| 源码位置 | `src/tools/ScheduleCronTool/CronCreateTool.ts` |

### 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `cron` | string | 是 | 5 字段 cron 表达式 (本地时区) |
| `prompt` | string | 是 | 要执行的提示词 |
| `recurring` | boolean | 否 | 是否循环执行 |
| `durable` | boolean | 否 | 是否持久化 |

### 提示词要点
- 5 字段 cron 语法,本地时区
- 一次性 vs 循环任务
- 持久化选项
- 添加抖动避免 :00/:30 的雷鸣群效应
- 可配置天数后自动过期

---

## CronDelete

| 属性 | 值 |
|------|------|
| 工具名称 | `CronDelete` |
| 源码位置 | `src/tools/ScheduleCronTool/CronDeleteTool.ts` |

### 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `id` | string | 是 | 要删除的 cron 任务 ID |

---

## CronList

| 属性 | 值 |
|------|------|
| 工具名称 | `CronList` |
| 源码位置 | `src/tools/ScheduleCronTool/CronListTool.ts` |

无输入参数。列出所有活跃的 cron 任务。
