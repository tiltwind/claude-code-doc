# Message

## Overview

A Message is an individual unit of communication within a conversation session. Messages can be from the user, the assistant (Claude), or the system. Assistant messages may contain both text content and tool use requests.

## Attributes

| Attribute | Description | Type | Required | Notes |
|-----------|-------------|------|----------|-------|
| id | Unique identifier for the message | text | Yes | Generated at creation |
| role | Who authored the message | reference to Message Role dictionary | Yes | — |
| content | The message content (text, tool use, tool result, images) | list of content blocks | Yes | Polymorphic content |
| timestamp | When the message was created | date | Yes | — |
| costTokens | Token count for this message | number | No | Estimated or actual |
| turnIndex | Sequential turn number in the conversation | number | Yes | For ordering |

### Content Block Types

| Block Type | Description | Applicable Roles |
|------------|-------------|-----------------|
| text | Plain text or markdown content | user, assistant, system |
| tool_use | Request to invoke a tool with parameters | assistant |
| tool_result | Result returned from a tool invocation | user (injected by system) |
| image | Image data (base64 or URL) | user |
| file | File attachment reference | user |

## State Definitions

Messages are immutable once created; they do not have lifecycle states.

## Relationships

| Related Model | Relationship Type | Description |
|---------------|-------------------|-------------|
| Session | many-to-one | Each message belongs to one session |
| Tool | reference | tool_use blocks reference a specific tool by name |
| Tool Result | one-to-one | Each tool_use block has a corresponding tool_result |
