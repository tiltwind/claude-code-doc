# Web Fetch Tool

## 基本信息

| 属性 | 值 |
|------|------|
| 工具名称 | `WebFetch` |
| 源码位置 | `src/tools/WebFetchTool/WebFetchTool.ts` |
| 类型 | 网页内容获取工具 |
| 延迟加载 | 是 |
| 只读 | 是 |
| 并发安全 | 是 |

## 功能描述

获取 URL 内容,将 HTML 转换为 markdown,通过小模型处理内容。

## 输入参数

| 参数 | 类型 | 必需 | 说明 |
|------|------|------|------|
| `url` | string | 是 | 要获取的 URL |
| `prompt` | string | 是 | 在获取内容上运行的提示词 |

## 输出结构

```json
{
  "bytes": 12345,
  "code": 200,
  "codeText": "OK",
  "result": "...",
  "durationMs": 500,
  "url": "..."
}
```

## 提示词要点

```
- 获取 URL 内容并使用小模型处理
- 15 分钟缓存
- 自动将 HTTP 升级为 HTTPS
- 处理重定向
- GitHub URL 优先使用 gh CLI
- 非预批准域名的引用限制 125 字符
- 认证/私有 URL 可能失败的警告
```

## 权限机制

- 预批准主机检查
- 基于主机名的权限规则 (allow/deny/ask)
- 权限更新建议
