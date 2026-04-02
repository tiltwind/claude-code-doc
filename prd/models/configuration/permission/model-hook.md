# Hook

## Overview

A Hook is a deterministic automation that executes a user-defined shell command in response to specific events in the Claude Code lifecycle. Hooks run without AI involvement and can allow, block, or modify operations.

## Attributes

| Attribute | Description | Type | Required | Notes |
|-----------|-------------|------|----------|-------|
| eventType | The event that triggers this hook | reference to Hook Event Type dictionary | Yes | — |
| command | Shell command to execute | text | Yes | Receives event data via stdin |
| toolName | Optional tool name filter (for tool use events) | text | No | Only trigger for specific tools |
| timeout | Maximum execution time in milliseconds | number | No | Default varies by event type |
| isEnabled | Whether this hook is currently active | boolean | Yes | Default: true |

## Relationships

| Related Model | Relationship Type | Description |
|---------------|-------------------|-------------|
| Settings | many-to-one | Hooks are defined within settings |
| Tool | reference | Tool use hooks can filter by tool name |
