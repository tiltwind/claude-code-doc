# PowerShell Tool

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `PowerShell` |
| 源码位置 | `src/tools/PowerShellTool/PowerShellTool.ts` |
| 启用条件 | `isPowerShellToolEnabled()` (Windows) |

## 功能描述

Bash 工具的 PowerShell 变体,用于 Windows 平台。

## 提示词要点

```
- 包含版本特定的指导 (Desktop 5.1 vs Core 7+)
- PowerShell 语法注意事项 (变量、转义、cmdlets、注册表、here-strings)
- 交互式命令警告
- 沙箱限制
- 应使用专用工具而非 PowerShell 进行文件操作
```
