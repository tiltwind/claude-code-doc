# Settings

## Overview

Settings represent the merged configuration that controls Claude Code behavior. Settings are loaded from multiple sources (user, project, local, flag, policy) and merged with later sources overriding earlier ones.

## Attributes

| Attribute | Description | Type | Required | Notes |
|-----------|-------------|------|----------|-------|
| permissions | Permission rules and default mode | structured | No | Contains allow, deny, ask lists |
| defaultPermissionMode | Default permission evaluation mode | reference to Permission Mode dictionary | No | — |
| environmentVariables | Custom environment variables for tool execution | map | No | Key-value pairs |
| mcpServers | MCP server configurations | map of MCP server configs | No | Keyed by server name |
| hooks | Hook definitions for automated behaviors | map of hook configs | No | Keyed by event type |
| additionalDirectories | Extra directories to include in context | list of text | No | — |
| model | Preferred Claude model | text | No | — |
| theme | Terminal color theme | text | No | — |
| customKeybindings | Custom keyboard shortcut overrides | list of keybinding configs | No | — |
| disableTools | List of tools to disable | list of text | No | — |
| source | Which settings source this was loaded from | reference to Settings Source dictionary | Yes | For merge tracking |

## Relationships

| Related Model | Relationship Type | Description |
|---------------|-------------------|-------------|
| Permission Rule | one-to-many | Settings contain permission rules |
| Hook | one-to-many | Settings define hook configurations |
| MCP Server | one-to-many | Settings configure MCP server connections |
