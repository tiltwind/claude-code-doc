# Plugin

## Overview

A Plugin is a third-party extension package that can deeply influence Claude Code's behavior — far beyond simply adding CLI commands. Plugins can modify the model's prompt, add tools, contribute skills, register hooks, configure MCP servers, adjust output styles, and set model/effort hints. This makes plugins a **model behavior layer extension**, not just a CLI plugin.

The plugin system covers the full lifecycle from loading to verification to marketplace management.

## Attributes

| Attribute | Description | Type | Required | Notes |
|-----------|-------------|------|----------|-------|
| name | Unique plugin identifier | text | Yes | — |
| version | Semantic version of the plugin | text | Yes | Supports auto-update |
| description | What the plugin provides | text | Yes | — |
| author | Plugin author or organization | text | No | — |
| tools | Tools provided by this plugin | list of reference to Tool | No | — |
| skills | Skills provided by this plugin (SKILL.md directories) | list of reference to Skill | No | — |
| markdownCommands | Markdown-based slash commands | list of text | No | — |
| hooks | Hook definitions (Pre/PostToolUse) | list of reference to Hook | No | Can intercept and modify tool behavior |
| outputStyles | Custom output rendering styles | list of structured | No | Modifies how results are displayed |
| mcpServers | MCP server configurations bundled with the plugin | list of MCP server configs | No | Plugin can bring its own tool servers |
| modelHints | Model preference hints | structured | No | Suggests which model to use |
| effortHints | Effort level hints | structured | No | Adjusts agent effort/thoroughness |
| runtimeVariables | Variables substituted at runtime (e.g., ${CLAUDE_PLUGIN_ROOT}) | map | No | For path resolution |
| isEnabled | Whether the plugin is currently active | boolean | Yes | Can be toggled |
| isBlocked | Whether the plugin is on the blocklist | boolean | No | Security measure |
| autoUpdate | Whether the plugin auto-updates | boolean | No | Default: depends on plugin config |

## Relationships

| Related Model | Relationship Type | Description |
|---------------|-------------------|-------------|
| Tool | one-to-many | Plugin can provide multiple tools |
| Skill | one-to-many | Plugin can provide SKILL.md directories |
| Hook | one-to-many | Plugin can register Pre/PostToolUse hooks |
| MCP Server | one-to-many | Plugin can bundle MCP server configurations |
| System Prompt | modifies | Plugin can influence the model's system prompt via tools, hooks, and hints |
