# Sleep Tool

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `Sleep` |
| 源码位置 | `src/tools/SleepTool/SleepTool.ts` |
| 启用条件 | `PROACTIVE` 或 `KAIROS` feature flag |

## 功能描述

等待指定的持续时间,可被用户中断。

## 提示词要点

```
- 优先于 Bash(sleep ...) 因为不占用 shell 进程
- 可被用户中断
- 响应周期性的 tick check-in
```
