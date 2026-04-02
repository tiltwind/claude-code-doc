# Skill

## Overview

A Skill is a named command (invoked via `/skill-name`) that extends Claude Code's functionality. Skills can be bundled with the CLI, loaded from the filesystem, provided by plugins, or converted from MCP tools. They expand into conversation prompts, execute local commands, or render interactive UI.

## Attributes

| Attribute | Description | Type | Required | Notes |
|-----------|-------------|------|----------|-------|
| name | Unique slash command name | text | Yes | e.g., "commit", "review" |
| description | Human-readable description of the skill | text | Yes | Shown in /help |
| source | Where the skill is loaded from | enum: bundled, filesystem, plugin, mcp | Yes | — |
| executionMode | How the skill is executed | reference to Skill Execution Mode dictionary | Yes | — |
| prompt | The prompt template or instructions | text | No | For prompt-based skills |
| filePath | Path to the skill definition file | text | No | For filesystem skills |
| isUserInvocable | Whether users can invoke this skill directly | boolean | Yes | Some skills are agent-only |
| triggerPatterns | Patterns that auto-trigger this skill | list of text | No | For automatic skill invocation |
| arguments | Accepted arguments schema | text | No | Freeform argument description |

## Relationships

| Related Model | Relationship Type | Description |
|---------------|-------------------|-------------|
| Plugin | many-to-one | Plugin-provided skills belong to a plugin |
| MCP Server | many-to-one | MCP-converted skills originate from a server |
| Tool | one-to-one | Skills are exposed as SkillTool to Claude |
