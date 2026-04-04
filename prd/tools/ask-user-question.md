# Ask User Question Tool

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `AskUserQuestion` |
| 源码位置 | `src/tools/AskUserQuestionTool/AskUserQuestionTool.tsx` |
| 类型 | 用户交互工具 |

## 功能描述

向用户提出多选题以收集信息、澄清歧义、理解偏好、做决定或提供选择。

## 输入参数

支持 1-4 个问题,每个问题有 2-4 个选项。

| 参数 | 类型 | 说明 |
|------|------|------|
| 问题 | array | 1-4 个问题 |
| 选项 | array | 每个问题 2-4 个选项 |
| label | string | 选项标签 |
| description | string | 选项描述 |
| preview | string | 可选预览内容 (支持 markdown 和 HTML) |
| multiSelect | boolean | 是否多选 |

## 提示词要点

```
- 向用户询问多选题以收集信息
- 支持 preview 字段用于并排比较 (markdown 和 HTML 变体)
- 支持多选
- 计划模式下的计划审批使用 ExitPlanMode,不使用此工具
```
