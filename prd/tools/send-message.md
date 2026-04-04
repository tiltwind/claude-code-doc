# Send Message Tool

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `SendMessage` |
| 源码位置 | `src/tools/SendMessageTool/SendMessageTool.ts` |
| 类型 | 代理间通信工具 |

## 功能描述

在队友代理间发送消息。支持点对点和广播通信。

## 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `to` | string | 是 | 接收者名称 ("*" 表示广播) |
| `summary` | string | 否 | 消息摘要 |
| `message` | string/object | 是 | 消息内容 |

### 结构化消息类型

| 类型 | 说明 |
|------|------|
| plain text | 普通文本消息 |
| `shutdown_request` | 关闭请求 |
| `shutdown_response` | 关闭响应 |
| `plan_approval_response` | 计划审批响应 |

## 提示词要点

```
- 按名称向队友发送消息,支持广播 ("*")
- 可选通过 UDS/bridge 进行跨会话通信
- 覆盖关闭和计划审批请求的协议响应
- 重要: 纯文本输出对其他代理不可见 — 必须使用此工具
```
