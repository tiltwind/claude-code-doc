# MCP Resource

## Overview

An MCP Resource is a named piece of content exposed by an MCP server that can be listed and read by Claude. Resources provide additional context without being executable tools.

## Attributes

| Attribute | Description | Type | Required | Notes |
|-----------|-------------|------|----------|-------|
| uri | Unique resource identifier | text | Yes | MCP resource URI format |
| name | Human-readable resource name | text | Yes | — |
| description | What the resource contains | text | No | — |
| mimeType | Content type of the resource | text | No | e.g., "text/plain", "application/json" |
| serverName | Name of the MCP server that provides this resource | text | Yes | — |

## Relationships

| Related Model | Relationship Type | Description |
|---------------|-------------------|-------------|
| MCP Server | many-to-one | Each resource belongs to one server |
