# Hook Event Type

## Overview

Events in the Claude Code lifecycle that can trigger user-defined hook commands.

## Values

| Value | Label | Description | Sort Order | Notes |
|-------|-------|-------------|------------|-------|
| pre-tool-use | Pre Tool Use | Fires before a tool is executed; can return permissionBehavior (allow/ask/deny), updatedInput (modify input), blockingError (block entirely), preventContinuation (stop flow), additionalContexts (inject context) | 1 | Most powerful hook type; but cannot bypass settings deny rules |
| post-tool-use | Post Tool Use | Fires after successful tool execution; can modify MCP tool output, add messages, inject additional context | 2 | Only runs on success (mutually exclusive with post-tool-use-failure) |
| post-tool-use-failure | Post Tool Use Failure | Fires after a tool execution fails | 3 | Only runs on failure (mutually exclusive with post-tool-use) |
| user-prompt-submit | User Prompt Submit | Fires when the user submits a prompt; can modify the prompt before processing | 4 | Can transform or block user input |
| file-changed | File Changed | Fires when a file is modified by Claude | 5 | Useful for auto-formatting |
| subagent-start | Subagent Start | Fires when a sub-agent is initialized | 6 | Registered via frontmatter hooks in agent definitions |
| stop | Stop Hook | Fires when the model stops outputting; decides whether to let the model continue | 7 | Controls multi-turn continuation behavior |
