# Grep Tool

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `Grep` |
| 源码位置 | `src/tools/GrepTool/GrepTool.ts` |
| 类型 | 内容搜索工具 |
| 只读 | 是 |
| 底层引擎 | ripgrep |

## 功能描述

基于 ripgrep 的强大搜索工具。始终使用 Grep 进行搜索任务,不要通过 Bash 调用 `grep` 或 `rg`。

## 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `pattern` | string | 是 | 正则表达式搜索模式 |
| `path` | string | 否 | 搜索路径 |
| `glob` | string | 否 | 文件过滤 glob 模式 |
| `type` | string | 否 | 文件类型 (如 js, py, rust) |
| `output_mode` | enum | 否 | `content` / `files_with_matches` / `count` |
| `-B` | number | 否 | 前文行数 |
| `-A` | number | 否 | 后文行数 |
| `-C` / `context` | number | 否 | 上下文行数 |
| `-n` | boolean | 否 | 显示行号 (默认 true) |
| `-i` | boolean | 否 | 忽略大小写 |
| `head_limit` | number | 否 | 限制输出条目数 (默认 250) |
| `offset` | number | 否 | 跳过前 N 条 |
| `multiline` | boolean | 否 | 多行匹配模式 |

## 输出结构

```json
{
  "mode": "files_with_matches",
  "numFiles": 10,
  "filenames": ["..."],
  "content": "...",
  "numLines": 42,
  "numMatches": 5,
  "appliedLimit": 250,
  "appliedOffset": 0
}
```

## 提示词要点

```
- 始终使用 Grep 进行搜索,永远不通过 Bash 调用 grep/rg
- 支持完整正则语法 (如 "log.*Error", "function\s+\w+")
- 用 glob 参数过滤文件 (如 "*.js", "**/*.tsx") 或 type 参数 (如 "js", "py")
- 输出模式: content 显示匹配行, files_with_matches 仅显示文件路径, count 显示匹配数
- 开放式搜索需多轮时使用 Agent 工具
- 模式语法用 ripgrep — 字面大括号需转义 (interface\{\})
- 多行匹配: 默认单行,跨行模式使用 multiline: true
```

## 实现细节

- 自动排除 VCS 目录
- 默认 head_limit 为 250
- 并发安全
