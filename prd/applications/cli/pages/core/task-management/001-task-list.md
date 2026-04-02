# Task List

## Page Description

Displays the current session's task list with status indicators, progress tracking, and dependency relationships. Tasks are shown inline in the conversation or in a dedicated view via `/tasks`.

**Entry points**: `/tasks` command; automatically shown when tasks are created by Claude

## Access Permissions

| Role | Access |
|------|--------|
| End User | View and manage tasks |
| Claude Assistant | Create, update, and query tasks via tools |

## Page Structure

### Section 1: Task List Header
- **Position**: Top of task display area
- **Content**:
  - Task count summary (e.g., "3/5 tasks completed")
  - Progress bar visualization
- **Interactions**: None (informational)

### Section 2: Task Items
- **Position**: Below header, vertical list
- **Content**:
  - Status icon: ○ (pending), ◑ (in_progress with spinner), ● (completed), ✕ (deleted)
  - Task subject text
  - Owner label (if assigned to specific agent)
  - Dependency indicators (blocked/blocking)
  - Active form text shown in spinner when in_progress
- **Interactions**:
  - Tasks are managed via Claude's tool invocations, not direct user interaction

## Feature Descriptions

### Feature 1: Task Progress Display
- **Trigger**: Task status changes
- **Business Logic**: Update task icon and progress bar; show spinner with activeForm for in_progress tasks
- **Related Models**: Task (status transitions)
- **Feedback**: Visual status change; progress bar updates

### Feature 2: Dependency Visualization
- **Trigger**: Tasks have blocks/blockedBy relationships
- **Business Logic**: Show dependency arrows or labels indicating which tasks block others
- **Related Models**: Task (blocks, blockedBy)
- **Feedback**: Blocked tasks show "Blocked by #N" label

## Data Display Rules

- **Default Sort**: Tasks ordered by creation time, grouped by status (in_progress first, then pending, then completed)
- **Empty State**: "No tasks" — shown when no tasks exist in the session
- **Completed Tasks**: Shown with strikethrough or dimmed text

## Page Navigation

| Destination | Trigger |
|-------------|---------|
| Conversation Main | Task list is inline within conversation; no explicit navigation |
