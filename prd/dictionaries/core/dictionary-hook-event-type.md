# Hook Event Type

## Overview

Events in the Claude Code lifecycle that can trigger user-defined hook commands.

## Values

| Value | Label | Description | Sort Order | Notes |
|-------|-------|-------------|------------|-------|
| pre-tool-use | Pre Tool Use | Fires before a tool is executed; can block or modify the invocation | 1 | Can return allow/block decisions |
| post-tool-use | Post Tool Use | Fires after a tool execution completes | 2 | Receives tool result |
| user-prompt-submit | User Prompt Submit | Fires when the user submits a prompt | 3 | Can modify the prompt before processing |
| file-changed | File Changed | Fires when a file is modified by Claude | 4 | Useful for auto-formatting |
