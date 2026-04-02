# Message Role

## Overview

Defines the roles that can author messages within a conversation session.

## Values

| Value | Label | Description | Sort Order | Notes |
|-------|-------|-------------|------------|-------|
| user | User | Message from the human user | 1 | Includes text, images, and file attachments |
| assistant | Assistant | Message from Claude AI | 2 | Includes text and tool use blocks |
| system | System | System-level message injected by the application | 3 | Used for instructions and context |
| attachment | Attachment | File or image attachment message | 4 | Treated as context for conversation |
| progress | Progress | Progress update during tool execution | 5 | Ephemeral; not persisted in history |
