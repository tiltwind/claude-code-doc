# Config Tool

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `Config` |
| 源码位置 | `src/tools/ConfigTool/ConfigTool.ts` |
| 启用条件 | Ant 内部用户 |

## 功能描述

获取或设置 Claude Code 配置设置。

## 提示词要点

```
- 动态从注册表构建设置列表
- 全局设置存储在 ~/.claude.json
- 项目设置存储在 settings.json
- 动态生成模型选项部分
```
