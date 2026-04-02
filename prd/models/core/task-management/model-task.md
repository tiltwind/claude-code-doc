# Task

## Overview

A Task is a trackable work item created during a Claude Code session to organize and monitor progress on multi-step work. Tasks support dependencies, ownership, and status tracking.

## Attributes

| Attribute | Description | Type | Required | Notes |
|-----------|-------------|------|----------|-------|
| taskId | Unique identifier for the task | text | Yes | Auto-generated |
| subject | Brief, actionable title in imperative form | text | Yes | e.g., "Fix auth bug in login flow" |
| description | Detailed description of what needs to be done | text | Yes | — |
| status | Current lifecycle state | reference to Task Status dictionary | Yes | Default: pending |
| activeForm | Present continuous form shown in spinner during in_progress | text | No | e.g., "Fixing auth bug" |
| owner | Agent name that owns this task | text | No | For multi-agent coordination |
| blocks | Task IDs that cannot start until this task completes | list of text | No | Dependency tracking |
| blockedBy | Task IDs that must complete before this task can start | list of text | No | Dependency tracking |
| metadata | Arbitrary key-value metadata | map | No | Extensible |
| createdAt | When the task was created | date | Yes | — |
| updatedAt | When the task was last modified | date | Yes | — |

## State Definitions

| State | Description | Notes |
|-------|-------------|-------|
| pending | Task created but work not started | Initial state |
| in_progress | Task is actively being worked on | Shows spinner in UI |
| completed | Task successfully finished | Terminal state |
| deleted | Task permanently removed | Terminal state; not visible |

## State Transitions

| Current State | Trigger Action | Target State | Preconditions | Post Actions |
|---------------|----------------|--------------|---------------|--------------|
| pending | Work begins on task | in_progress | No blocking tasks are in_progress or pending | Show spinner with activeForm |
| pending | Task no longer needed | deleted | — | Remove from task list |
| in_progress | Work completed successfully | completed | — | Mark as done; unblock dependent tasks |
| in_progress | Task abandoned or superseded | deleted | — | Remove from task list |
| completed | (no transitions) | — | — | Terminal state |

## Relationships

| Related Model | Relationship Type | Description |
|---------------|-------------------|-------------|
| Session | many-to-one | Each task belongs to one session |
| Task (blocks) | many-to-many | Tasks can block other tasks |
| Task (blockedBy) | many-to-many | Tasks can be blocked by other tasks |
