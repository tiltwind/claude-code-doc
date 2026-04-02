# Product Architecture

## Product Overview

Claude Code is a terminal-based AI coding assistant that combines conversational AI with deep development environment integration. The architecture is organized around a conversation loop where user messages are processed by Claude, which can invoke tools to interact with the local environment, and results are streamed back to the user.

## Platform Entry Points

Claude Code is not a single-entry application. It exposes multiple entry points serving different platforms from the same agent runtime:

| Entry Point | Purpose | Loading Strategy |
|-------------|---------|-----------------|
| cli.tsx | CLI terminal interface | Fast-path dispatch: --version (zero-load), remote-control (bridge mode), daemon, ps/logs/attach/kill (session management), --worktree --tmux (exec into tmux). Only falls through to main.tsx if no fast-path matches. |
| mcp.ts | MCP protocol server | Exposes Claude Code as an MCP server for SDK consumers |
| sdk/ | Agent SDK integration | Programmatic API for building custom agents |
| init.ts | Project initialization | Setup CLAUDE.md, skills, hooks |

All imports are dynamic (lazy-loaded) to minimize startup time — a critical product decision since the CLI is invoked millions of times daily, and fast-paths like `--version` must not load the full runtime.

## Business Modules

### Core

Core modules handle the fundamental conversation and interaction loop.

#### Conversation Engine
- **Responsibilities**: Manage multi-turn conversations with Claude, handle message streaming, context window management, and conversation history
- **Features**:
  - Multi-turn conversation with streaming responses
  - Context window optimization and message compaction
  - Conversation history persistence and session resume
  - Plan mode for structured thinking with approval gates
  - Brief mode for condensed interaction style
  - Token estimation and cost tracking
- **Related Models**: Session, Message, ConversationHistory
- **Related Processes**: Conversation Flow, Message Compaction

#### Tool Execution System
- **Responsibilities**: Register, discover, and execute tools that Claude can invoke to interact with the development environment
- **Features**:
  - 42+ built-in tools (file I/O, shell, search, web, etc.)
  - 14-step execution pipeline (find tool → validate → classify → hook → permission → execute → trace → result)
  - Streaming Tool Execution: tools begin executing as soon as their tool_use block is complete, even while the model is still generating the next tool_use — significantly reducing latency for multi-tool turns
  - Fail-closed tool defaults: new tools are assumed non-concurrent-safe, writable, and permission-requiring unless explicitly declared otherwise
  - Speculative classifier: for BashTool, a risk classifier runs in parallel with hooks to pre-compute permission decisions
  - Tool search and discovery
  - MCP tool integration
- **Related Models**: Tool, ToolResult
- **Related Processes**: Tool Execution Flow, Permission Check

#### Prompt System
- **Responsibilities**: Assemble, cache, and optimize the system prompt sent to Claude on each API call, balancing completeness with token economy
- **Features**:
  - Static sections (cacheable): identity, system rules, task behavior rules, risk action rules, tool usage syntax, tone/style, output efficiency
  - Dynamic sections (per-turn): session guidance, CLAUDE.md memory, environment info (OS, shell, cwd, model), language preferences, MCP server instructions, function result clearing, token budget hints
  - SYSTEM_PROMPT_DYNAMIC_BOUNDARY marker separating static from dynamic sections for API-level prompt caching
  - Prompt Section Registry: sections created via systemPromptSection() are cached until /clear or /compact; only DANGEROUS_uncachedSystemPromptSection() sections recompute each turn (e.g., MCP instructions that change between turns)
  - Each tool has its own prompt() method that dynamically generates its description based on current available tools and permission mode
- **Related Models**: SystemPrompt
- **Related Processes**: Prompt Assembly

#### Permission & Safety
- **Responsibilities**: Enforce safety guardrails on tool execution through layered permission rules, dangerous pattern detection, and classifier-based auto-approval
- **Features**:
  - Three-layer safety net architecture:
    1. **Speculative Classifier** (pre-judgment): BashTool risk classifier runs in parallel with hooks, providing early risk assessment without blocking the main flow
    2. **Hook Policy Layer**: PreToolUse hooks can return allow/ask/deny decisions, modify input, inject context, or block entirely — but Hook allow cannot bypass settings deny rules
    3. **Permission Decision**: Combines rule configuration, classifier results, and user interaction for the final allow/reject decision
  - Permission modes (Default, AcceptEdits, Auto, Plan)
  - Per-tool allow/deny/ask rules with source priority (policy > flag > local > project > user)
  - Dangerous pattern detection (command injection, destructive ops)
  - Classifier-based auto-approval with safety checks
  - Denial tracking and fallback prompting
  - Enterprise policy enforcement
  - resolveHookPermissionDecision: critical glue ensuring Hook allow + settings deny → deny takes precedence; Hook allow + settings ask → still prompts user
- **Related Models**: PermissionRule, PermissionDecision
- **Related Processes**: Permission Check Flow

### Extension

Modules that extend Claude Code's capabilities beyond built-in tools.

#### Skill System
- **Responsibilities**: Provide a mechanism for users and teams to define custom slash commands that expand into prompts or execute local actions
- **Features**:
  - Bundled skills (shipped with CLI)
  - Filesystem skills (.claude/skills/ directory)
  - Plugin-provided skills
  - MCP-converted skills
  - Inline and forked execution modes
- **Related Models**: Skill
- **Related Processes**: Skill Invocation

#### Plugin System
- **Responsibilities**: Enable third-party extensions to add tools, commands, and capabilities
- **Features**:
  - Plugin manifest and versioning
  - Plugin loading and lifecycle management
  - Plugin-provided capabilities: markdown commands, SKILL.md directories, hooks (Pre/PostToolUse), output styles, MCP server configurations, model and effort hints
  - Runtime variable substitution (e.g., ${CLAUDE_PLUGIN_ROOT})
  - Auto-update, version management, blocklist
  - Plugins can modify model behavior: change prompts, add tools, modify permission rules
- **Related Models**: Plugin
- **Related Processes**: Plugin Loading

#### MCP Integration
- **Responsibilities**: Connect to external tool servers via the Model Context Protocol, enabling dynamic tool and resource discovery
- **Features**:
  - MCP server connection management
  - Dynamic tool discovery from servers
  - Resource listing and reading
  - OAuth authentication for MCP servers
  - Per-server allowlist/denylist
- **Related Models**: MCPServer, MCPResource
- **Related Processes**: MCP Server Connection

#### Agent & Collaboration
- **Responsibilities**: Orchestrate multi-agent workflows including sub-agents, teammate agents, and coordinator-worker patterns
- **Features**:
  - 6 built-in agent types: General Purpose (full tools), Explore (read-only code exploration), Plan (planning without execution), Verification (adversarial testing), Claude Code Guide (usage guidance), Statusline Setup (status bar config)
  - Sub-agent spawning with separate budgets and fork-path cache optimization (inherits parent system prompt byte-for-byte to maximize prompt cache hit rate)
  - Teammate agents (persistent, task-assignable)
  - Coordinator mode for multi-worker orchestration
  - Agent swarm initialization
  - Inter-agent messaging
  - 5 task types: DreamTask (autonomous background), LocalAgentTask (foreground/background/async), RemoteAgentTask (remote execution), InProcessTeammateTask (in-process teammate), LocalShellTask (shell process)
  - Agent lifecycle management: MCP server init, file state cloning, permission mode setup, tool pool assembly, hook registration, skill preloading, transcript recording, cleanup chain (MCP connections, hooks, tracing, shell tasks, todos)
- **Related Models**: Agent, Team
- **Related Processes**: Agent Spawning, Coordinator Workflow

### Configuration

Modules managing user preferences, settings, and authentication.

#### Settings & Configuration
- **Responsibilities**: Manage multi-layered settings from user, project, local, flag, and policy sources with proper merging and override semantics
- **Features**:
  - Global user settings (~/.claude/settings.json)
  - Project settings (.claude/settings.json)
  - Local gitignored settings (.claude/local-settings.json)
  - Enterprise managed settings (MDM / remote API)
  - CLAUDE.md project instructions
  - Keybinding customization
  - Theme management
- **Related Models**: Settings
- **Related Processes**: Settings Resolution

#### Authentication
- **Responsibilities**: Handle user identity, API key management, OAuth flows, and subscription verification
- **Features**:
  - API key authentication
  - OAuth (Claude.ai, GitHub, Slack, Google)
  - macOS Keychain integration
  - AWS STS identity verification
  - Subscription tier detection
- **Related Models**: AuthToken
- **Related Processes**: Authentication Flow

#### Model Selection
- **Responsibilities**: Resolve the active Claude model based on session overrides, startup flags, environment variables, and user settings
- **Features**:
  - Model switching (/model command)
  - Model aliases and parsing
  - 1M context variants
  - Pricing information display
- **Related Models**: ModelConfig
- **Related Processes**: Model Resolution

### DevOps

Modules supporting development operations and workflow automation.

#### Git Workflow
- **Responsibilities**: Integrate with git for status tracking, commit creation, branch management, and PR workflows
- **Features**:
  - Git status and branch information
  - AI-assisted commit message generation
  - End-to-end commit-push-PR workflow
  - Code review (/review, /ultrareview)
  - Security review (/security-review)
- **Related Models**: (uses git directly)
- **Related Processes**: Commit Flow, PR Creation, Code Review

#### Task Management
- **Responsibilities**: Provide built-in task/todo tracking for organizing work within conversations
- **Features**:
  - Task creation, update, and status tracking
  - Task dependencies (blocks/blockedBy)
  - Task list display with progress indicators
  - Scheduled tasks (cron-triggered)
- **Related Models**: Task
- **Related Processes**: Task Lifecycle

#### Hook System
- **Responsibilities**: Enable deterministic automation by executing user-defined shell commands in response to tool events
- **Features**:
  - Pre/post tool use hooks
  - User prompt submit hooks
  - File change hooks
  - HTTP webhook hooks
- **Related Models**: Hook
- **Related Processes**: Hook Execution

### Input & Output

Modules handling user input and output rendering.

#### Terminal UI
- **Responsibilities**: Render the conversational interface in the terminal using React/Ink, handling input, output formatting, and interactive elements
- **Features**:
  - Streaming markdown rendering
  - Syntax-highlighted code blocks
  - Interactive permission dialogs
  - Progress spinners and status indicators
  - Virtual scrolling for large outputs
  - Vim keybinding support
- **Related Models**: (UI components)
- **Related Processes**: (rendering pipeline)

#### Voice Input
- **Responsibilities**: Capture audio input and convert speech to text for hands-free interaction
- **Features**:
  - Push-to-talk recording
  - Streaming speech-to-text
  - Audio level visualization
  - Keyword detection
- **Related Models**: (audio state)
- **Related Processes**: Voice Input Flow

#### Remote Session
- **Responsibilities**: Sync CLI sessions with the claude.ai web interface for cross-device access
- **Features**:
  - WebSocket-based session sync
  - Remote permission request handling
  - Bridge mode connection management
  - Trusted device tokens
- **Related Models**: RemoteSession
- **Related Processes**: Remote Session Sync

### Observability

Modules for monitoring, analytics, and cost tracking.

#### Telemetry & Analytics
- **Responsibilities**: Track usage, performance, and costs across the application for observability, debugging, and product analytics
- **Features**:
  - Datadog integration for metrics
  - Perfetto tracing for performance profiling
  - OpenTelemetry (OTel) event tracking
  - Cost tracker: API call cost computation integrated into runtime
  - Rate limit management and monitoring
  - GrowthBook feature flag evaluation
  - Privacy-aware telemetry filtering
- **Related Models**: (telemetry events)
- **Related Processes**: (observability pipeline)

## Business Module Relationship Diagram

```mermaid
graph TB
    EP[Entry Points: cli.tsx / mcp.ts / sdk/]

    subgraph Core["Core"]
        CE[Conversation Engine]
        PR[Prompt System]
        TES[Tool Execution System]
        PS[Permission & Safety<br/>Three-Layer Safety Net]
    end

    subgraph Extension["Extension"]
        SK[Skill System]
        PL[Plugin System]
        MCP[MCP Integration]
        AG[Agent & Collaboration<br/>6 Agent Types]
    end

    subgraph Configuration["Configuration"]
        SC[Settings & Configuration]
        AU[Authentication]
        MS[Model Selection]
    end

    subgraph DevOps["DevOps"]
        GW[Git Workflow]
        TM[Task Management]
        HK[Hook System]
    end

    subgraph IO["Input & Output"]
        TUI[Terminal UI]
        VI[Voice Input]
        RS[Remote Session / Bridge]
    end

    subgraph Observability["Observability"]
        TA[Telemetry & Analytics]
    end

    EP -->|dispatches| CE
    CE -->|assembles| PR
    CE -->|invokes| TES
    TES -->|three-layer check| PS
    PS -->|layer 2: hooks| HK
    TES -->|loads from| SK
    TES -->|loads from| PL
    TES -->|loads from| MCP
    TES -->|spawns| AG
    CE -->|uses| MS
    CE -->|requires| AU
    PS -->|reads| SC
    PL -->|extends| PR
    MCP -->|injects instructions| PR
    CE -->|renders via| TUI
    VI -->|feeds| CE
    RS -->|syncs| CE
    GW -->|uses| TES
    TM -->|managed via| TES
    AG -->|coordinates| TES
    TES -->|emits| TA
    CE -->|tracks cost| TA
```
