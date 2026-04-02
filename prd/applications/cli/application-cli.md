# Claude Code CLI Application

## Application Overview

Claude Code CLI is a terminal-based interactive application that provides a conversational interface for developers to interact with Claude AI. It renders a rich terminal UI using React/Ink with streaming markdown, syntax highlighting, interactive dialogs, and progress indicators. The application runs in both standard terminals and integrated terminal panes within IDEs.

## Target User Roles

| Role | Usage Context |
|------|---------------|
| End User (Developer) | Primary user; interacts via keyboard input in the terminal |
| Enterprise Administrator | Configures settings and policies that affect the CLI experience |

## Page Framework

### Navigation Structure

The CLI uses a **single-screen conversation layout** with modal overlays for secondary interactions:

- **Main Screen**: Conversation view with input area at bottom
- **Status Line**: Top bar showing model, cost, session info
- **Toolbar**: Bottom bar showing available actions and keybindings
- **Modal Overlays**: Permission dialogs, file pickers, command palette
- **Slash Command Routing**: `/command` input routes to specific command pages

### Page Map

| Page | Path | Description | Module |
|------|------|-------------|--------|
| Conversation | core/conversation/001-conversation-main.md | Main conversation interface | Conversation |
| Permission Dialog | core/conversation/002-permission-dialog.md | Tool permission approval/denial | Permission |
| Task List | core/task-management/001-task-list.md | Session task tracking display | Task Management |
| Settings | configuration/settings/001-settings-view.md | Settings and configuration management | Settings |
| Commit | devops/git-workflow/001-commit-view.md | Git commit workflow interface | Git Workflow |

### Global Components

| Component | Description |
|-----------|-------------|
| Status Line | Top bar: model name, token usage, cost estimate, session URL (if remote) |
| Input Area | Multi-line text input with history, vim mode, paste handling, voice indicator |
| Toolbar | Bottom bar: keybinding hints, permission mode indicator, brief mode toggle |
| Spinner | Shows active tool name or task activeForm during processing |
| Markdown Renderer | Streams and formats Claude's markdown responses with syntax highlighting |
| Diff Viewer | Shows file changes with add/remove highlighting |
