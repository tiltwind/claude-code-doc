# Conversation History

## Overview

A persisted record of a session's messages, enabling session resume, history browsing, and conversation recovery. Stored on the local filesystem.

## Attributes

| Attribute | Description | Type | Required | Notes |
|-----------|-------------|------|----------|-------|
| sessionId | Reference to the original session | text | Yes | Used for resume |
| projectPath | Working directory of the session | text | Yes | Associates history with project |
| messages | Complete ordered list of messages | list of reference to Message | Yes | Serialized to disk |
| model | Model used in the session | text | Yes | — |
| createdAt | When the session started | date | Yes | — |
| updatedAt | When the session was last active | date | Yes | — |
| leafMessageId | ID of the most recent message | text | Yes | For quick resume |
| summary | Optional auto-generated session summary | text | No | For history browsing |

## Relationships

| Related Model | Relationship Type | Description |
|---------------|-------------------|-------------|
| Session | one-to-one | Each session has one history record |
| Message | one-to-many | History contains all messages from the session |
