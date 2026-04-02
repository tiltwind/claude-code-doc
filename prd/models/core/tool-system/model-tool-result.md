# Tool Result

## Overview

The output produced by a tool invocation, containing the result data, status, and any error information. Tool results are injected back into the conversation as tool_result content blocks.

## Attributes

| Attribute | Description | Type | Required | Notes |
|-----------|-------------|------|----------|-------|
| toolUseId | Reference to the tool_use block that triggered this result | text | Yes | Correlates request and response |
| toolName | Name of the tool that was invoked | text | Yes | — |
| content | The result data (text, images, or structured data) | list of content blocks | Yes | Polymorphic |
| isError | Whether the tool execution failed | boolean | Yes | — |
| errorMessage | Human-readable error description | text | No | Set when isError is true |
| duration | Execution time in milliseconds | number | No | For performance tracking |

## Relationships

| Related Model | Relationship Type | Description |
|---------------|-------------------|-------------|
| Tool | many-to-one | Each result is produced by one tool |
| Message | part-of | Results are embedded in user messages as tool_result blocks |
