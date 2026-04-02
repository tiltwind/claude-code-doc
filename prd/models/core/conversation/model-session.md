# Session

## Overview

A Session represents a single conversation between the user and Claude within Claude Code. It encapsulates the full lifecycle from initialization through active conversation to termination, including all messages, tool invocations, context, and configuration state.

## Attributes

| Attribute | Description | Type | Required | Notes |
|-----------|-------------|------|----------|-------|
| sessionId | Unique identifier for the session | text | Yes | Generated at creation |
| workingDirectory | The primary working directory for this session | text | Yes | Determines project context |
| model | The Claude model used for this session | reference to Model Tier dictionary | Yes | Can be changed mid-session |
| permissionMode | Active permission evaluation mode | reference to Permission Mode dictionary | Yes | Default: "default" |
| state | Current lifecycle state of the session | reference to Session State dictionary | Yes | — |
| messages | Ordered list of messages in the conversation | list of reference to Message | Yes | Grows over session lifetime |
| tasks | Tasks created during this session | list of reference to Task | No | Managed via task tools |
| tokenUsage | Cumulative token usage (input + output) | number | Yes | Updated per turn |
| costEstimate | Estimated API cost in USD | number | Yes | Updated per turn |
| startedAt | When the session began | date | Yes | — |
| endedAt | When the session ended | date | No | Set on termination |
| parentSessionId | ID of the parent session (if sub-agent) | text | No | For agent hierarchy |
| additionalDirectories | Extra directories added to context | list of text | No | Via /add-dir command |
| isRemote | Whether synced with claude.ai | boolean | No | Requires bridge entitlement |
| isBriefMode | Whether brief mode is active | boolean | No | Condensed output style |
| isPlanMode | Whether plan mode is active | boolean | No | Read-only tool access |

## State Definitions

| State | Description | Notes |
|-------|-------------|-------|
| initializing | Loading context, settings, and preparing the conversation | Reads CLAUDE.md, git status, etc. |
| active | Ready for user input; conversation is ongoing | Normal operating state |
| processing | Claude is generating a response or executing tools | User can cancel with Escape/Ctrl-C |
| compacting | Message history is being compressed to fit context window | Triggered automatically |
| backgrounded | Session is running in the background | Can be resumed later |
| ended | Session has been terminated | Final state |

## State Transitions

| Current State | Trigger Action | Target State | Preconditions | Post Actions |
|---------------|----------------|--------------|---------------|--------------|
| initializing | Context loaded successfully | active | Settings and context resolved | Display welcome message |
| active | User submits a prompt | processing | Prompt is non-empty | Send to Claude API |
| processing | Claude response complete | active | All tool executions finished | Render response |
| processing | User cancels | active | — | Abort current API call |
| active | Context limit approaching | compacting | Token count exceeds threshold | — |
| compacting | Compaction complete | active | Messages successfully compressed | Resume conversation |
| active | User backgrounds session | backgrounded | — | Persist session state |
| backgrounded | User resumes session | active | Session file exists | Reload state |
| active | User exits or session ends | ended | — | Save conversation history |
| processing | User exits | ended | — | Abort and save |

## Relationships

| Related Model | Relationship Type | Description |
|---------------|-------------------|-------------|
| Message | one-to-many | A session contains an ordered list of messages |
| Task | one-to-many | A session may contain tasks for tracking work |
| Session (parent) | many-to-one | Sub-agent sessions reference a parent session |
| Conversation History | one-to-one | Session state is persisted as conversation history |
