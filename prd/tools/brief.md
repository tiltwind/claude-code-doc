# Brief Tool (SendUserMessage)

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `SendUserMessage` |
| 源码位置 | `src/tools/BriefTool/BriefTool.ts` |
| 类型 | 用户通信工具 |

## 功能描述

向用户发送消息的主要通道。此工具外的文本仅在详细视图中可见。支持 markdown 消息和文件附件。

## 状态字段

| 状态 | 说明 |
|------|------|
| `normal` | 正常消息 |
| `proactive` | 主动消息 |

## 提示词要点

```
- 主要用户通信渠道
- 此工具外的文本仅在详细视图中可见
- 支持 markdown 消息和文件附件
- proactive 模式强调始终通过此工具回复
```
