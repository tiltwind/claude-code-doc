# Plugin

## Overview

A Plugin is a third-party extension package that adds tools, commands, and capabilities to Claude Code. Plugins are versioned and managed through a registration system.

## Attributes

| Attribute | Description | Type | Required | Notes |
|-----------|-------------|------|----------|-------|
| name | Unique plugin identifier | text | Yes | — |
| version | Semantic version of the plugin | text | Yes | — |
| description | What the plugin provides | text | Yes | — |
| author | Plugin author or organization | text | No | — |
| tools | Tools provided by this plugin | list of reference to Tool | No | — |
| skills | Skills provided by this plugin | list of reference to Skill | No | — |
| isEnabled | Whether the plugin is currently active | boolean | Yes | Can be toggled |

## Relationships

| Related Model | Relationship Type | Description |
|---------------|-------------------|-------------|
| Tool | one-to-many | Plugin can provide multiple tools |
| Skill | one-to-many | Plugin can provide multiple skills |
