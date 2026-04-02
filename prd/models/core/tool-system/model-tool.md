# Tool

## Overview

A Tool is a capability that Claude can invoke to interact with the development environment. Tools are the primary mechanism through which Claude performs actions like reading files, running commands, or searching the web. Tools are registered at startup and may come from built-in sources, MCP servers, or plugins.

The tool system follows a **fail-closed** design principle: if a new tool forgets to declare its safety properties, the system defaults to the most restrictive settings — assumed non-concurrent-safe, writable, and permission-requiring. This prevents the common failure mode where a forgotten configuration leads to a dangerous tool executing without permission checks.

## Attributes

| Attribute | Description | Type | Required | Notes |
|-----------|-------------|------|----------|-------|
| name | Unique identifier for the tool | text | Yes | Used in tool_use blocks |
| displayName | Human-readable name shown in UI | text | Yes | — |
| description | Describes what the tool does (shown to Claude) | text | Yes | Influences Claude's tool selection |
| category | Functional grouping | reference to Tool Category dictionary | Yes | — |
| inputSchema | Zod schema for input validation (first-pass validation) | structured (Zod schema) | Yes | Used in pipeline step 3 |
| source | Where the tool is registered from | enum: built-in, mcp, plugin | Yes | — |
| isReadOnly | Whether the tool only reads and never modifies state | boolean | Yes | **Fail-closed default: false** (assumed writable). Affects plan mode availability |
| isDestructive | Whether the tool can perform destructive/irreversible operations | boolean | Yes | **Fail-closed default: true** (assumed destructive). Triggers stricter permission checks |
| isConcurrencySafe | Whether the tool can be safely executed in parallel with other tools | boolean | Yes | **Fail-closed default: false** (assumed not safe for parallel execution) |
| requiresPermission | Whether the tool requires permission checks before execution | boolean | Yes | **Fail-closed default: true** (goes through full permission pipeline) |
| isAsync | Whether the tool supports streaming output | boolean | No | — |
| agentTypes | Which agent types can use this tool | list of text | No | Restricts tool availability |

## Key Methods

Each tool implements these methods that participate in the 14-step execution pipeline:

| Method | Description | Pipeline Step |
|--------|-------------|--------------|
| call() | Executes the tool with validated input | Step 10 |
| validateInput() | Fine-grained validation beyond Zod schema (e.g., path validation, argument checking) | Step 4 |
| checkPermissions() | Tool-level permission checking logic | Step 8 |
| preparePermissionMatcher() | Pre-processing for hook pattern matching | Step 6 |
| prompt() | Dynamically generates the tool's description for the system prompt based on current tools and permission mode | Prompt assembly |
| backfillObservableInput() | Populates observable fields in SDK stream and transcript | Step 11 |
| toAutoClassifierInput() | Generates compact representation for the speculative safety classifier | Step 5 |
| render methods (6+) | Handle UI display of tool progress, results, errors, denials, and grouping | UI rendering |

## Relationships

| Related Model | Relationship Type | Description |
|---------------|-------------------|-------------|
| Tool Result | one-to-many | Each tool invocation produces a result |
| Permission Rule | many-to-many | Tools are subject to permission rules |
| MCP Server | many-to-one | MCP-sourced tools belong to a server |
| Plugin | many-to-one | Plugin-sourced tools belong to a plugin |
