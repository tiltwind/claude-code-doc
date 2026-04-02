# Tool

## Overview

A Tool is a capability that Claude can invoke to interact with the development environment. Tools are the primary mechanism through which Claude performs actions like reading files, running commands, or searching the web. Tools are registered at startup and may come from built-in sources, MCP servers, or plugins.

## Attributes

| Attribute | Description | Type | Required | Notes |
|-----------|-------------|------|----------|-------|
| name | Unique identifier for the tool | text | Yes | Used in tool_use blocks |
| displayName | Human-readable name shown in UI | text | Yes | — |
| description | Describes what the tool does (shown to Claude) | text | Yes | Influences Claude's tool selection |
| category | Functional grouping | reference to Tool Category dictionary | Yes | — |
| parameters | Schema defining accepted input parameters | structured (JSON Schema) | Yes | Validated before execution |
| source | Where the tool is registered from | enum: built-in, mcp, plugin | Yes | — |
| isReadOnly | Whether the tool only reads and never modifies state | boolean | Yes | Affects plan mode availability |
| requiresPermission | Whether the tool requires permission checks before execution | boolean | Yes | False for read-only tools in default mode |
| isAsync | Whether the tool supports streaming output | boolean | No | — |
| agentTypes | Which agent types can use this tool | list of text | No | Restricts tool availability |

## Relationships

| Related Model | Relationship Type | Description |
|---------------|-------------------|-------------|
| Tool Result | one-to-many | Each tool invocation produces a result |
| Permission Rule | many-to-many | Tools are subject to permission rules |
| MCP Server | many-to-one | MCP-sourced tools belong to a server |
| Plugin | many-to-one | Plugin-sourced tools belong to a plugin |
