# Task Type

## Overview

Defines the types of background/foreground tasks that the agent system can manage. Each type represents a different execution context with distinct lifecycle management, abort handling, and resource cleanup behaviors.

## Values

| Value | Label | Description | Sort Order | Notes |
|-------|-------|-------------|------------|-------|
| dream-task | Dream Task | Autonomous background task that runs independently | 1 | Self-directed; no parent supervision |
| local-agent-task | Local Agent Task | Local agent execution task supporting foreground, background, and async modes | 2 | Most common type; used by AgentTool |
| remote-agent-task | Remote Agent Task | Agent task executing on a remote environment | 3 | Used for remote/bridge sessions |
| in-process-teammate-task | In-Process Teammate Task | Teammate agent running within the same process | 4 | Visible in UI sidebar; persistent |
| local-shell-task | Local Shell Task | Shell command process task | 5 | For long-running shell processes |

Background agents have independent abort controllers, can continue running after the parent foreground completes, and notify the main thread upon completion. Foreground agents can be converted to background mid-execution. This lifecycle management is critical for productization — handling interruption, dirty state cleanup, and session recovery.
