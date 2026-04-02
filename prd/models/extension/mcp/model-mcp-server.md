# MCP Server

## Overview

An MCP Server is an external service connected via the Model Context Protocol that dynamically provides tools and resources to Claude Code. Servers are configured per-project or globally and support OAuth authentication.

## Attributes

| Attribute | Description | Type | Required | Notes |
|-----------|-------------|------|----------|-------|
| name | Unique server identifier | text | Yes | Used in configuration |
| command | Command to start the server process | text | Yes | e.g., "npx mcp-server-xyz" |
| args | Arguments passed to the server command | list of text | No | — |
| env | Environment variables for the server process | map | No | — |
| connectionStatus | Current connection state | enum: disconnected, connecting, connected, error | Yes | — |
| tools | Tools discovered from this server | list of reference to Tool | No | Populated on connection |
| resources | Resources exposed by this server | list of reference to MCP Resource | No | Populated on connection |
| allowedTools | Allowlist of tool names (if restricted) | list of text | No | Empty means all allowed |
| deniedTools | Denylist of tool names | list of text | No | Takes precedence over allow |
| requiresAuth | Whether OAuth authentication is needed | boolean | No | — |

## State Definitions

| State | Description | Notes |
|-------|-------------|-------|
| disconnected | Server is not running | Initial state |
| connecting | Server process is starting | Waiting for handshake |
| connected | Server is active and tools are available | Normal operating state |
| error | Server failed to start or lost connection | Shows error details |

## State Transitions

| Current State | Trigger Action | Target State | Preconditions | Post Actions |
|---------------|----------------|--------------|---------------|--------------|
| disconnected | User starts server or session init | connecting | Server config exists | Start server process |
| connecting | Handshake successful | connected | Server responds to init | Discover tools and resources |
| connecting | Handshake failed or timeout | error | — | Show error to user |
| connected | Server process exits | disconnected | — | Mark tools as unavailable |
| connected | Connection lost | error | — | Attempt reconnection |
| error | User retries or auto-reconnect | connecting | — | Restart server process |

## Relationships

| Related Model | Relationship Type | Description |
|---------------|-------------------|-------------|
| Tool | one-to-many | Server provides discovered tools |
| MCP Resource | one-to-many | Server exposes resources |
| Skill | one-to-many | Server tools can be converted to skills |
