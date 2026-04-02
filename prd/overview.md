# Claude Code

## Product Positioning

Claude Code is Anthropic's official command-line interface (CLI) for Claude AI, designed for software engineers to interact with Claude directly from their terminal. It bridges the gap between AI-powered code generation and real-world development workflows by providing Claude with deep access to local development environments — file systems, shell commands, git repositories, and IDE integrations — enabling it to act as an intelligent coding assistant embedded in the developer's daily workflow.

## Target Users

| User Group | Key Characteristics |
|------------|---------------------|
| Individual Developers | Software engineers seeking AI-assisted coding, debugging, refactoring, and code review directly in the terminal |
| Enterprise Teams | Organizations deploying Claude Code with managed settings, policy enforcement, and centralized configuration |
| DevOps / Platform Engineers | Engineers automating workflows via hooks, scheduled tasks, and CI/CD integration |
| Open Source Contributors | Developers using Claude Code for code review, PR workflows, and project onboarding |
| Integration Partners | Developers building on top of Claude via MCP servers, plugins, and the Agent SDK |

## Core Value Proposition

| Value | Description |
|-------|-------------|
| Deep Environment Access | Claude can read/write files, execute shell commands, search codebases, and interact with git — operating within the developer's actual project context |
| Conversational Coding | Multi-turn conversations with streaming responses, plan mode for structured thinking, and brief mode for efficiency |
| End-to-End Dev Workflow | From code generation to commit, push, PR creation, and code review — all within a single CLI session |
| Extensibility | Skills, plugins, MCP servers, and hooks allow teams to customize and extend Claude Code for their specific workflows |
| Safety & Control | Layered permission system with dangerous pattern detection, per-tool rules, and enterprise policy enforcement |
| Multi-Agent Collaboration | Sub-agents, teammate agents, coordinator mode, and agent swarms for parallelizing complex tasks |
| IDE Integration | Direct connections to VS Code, JetBrains, and other editors for seamless code navigation and editing |

## Design Principles

Seven design principles extracted from the architecture, each backed by concrete implementation in the codebase:

| # | Principle | Description | Implementation Example |
|---|-----------|-------------|----------------------|
| 1 | **Don't Trust Model Spontaneity** | Desired behaviors are codified into system prompts and runtime checks, not left to the model's improvisation. Behavioral rules are written as institution, not suggestion. | getSimpleDoingTasksSection() and getActionsSection() — explicit "do/don't" rules in system prompt |
| 2 | **Separate Roles** | At minimum, separate "doers" from "validators". Even when using the same model, splitting responsibilities dramatically improves quality. | Verification Agent (adversarial tester), Explore Agent (read-only), Plan Agent (no execution) |
| 3 | **Tool Invocation Requires Governance** | The model requesting a tool doesn't mean it executes immediately. Every invocation passes through input validation, risk classification, hooks, and permission decisions. | toolExecution.ts 14-step pipeline |
| 4 | **Context Is Budget** | Every token costs money and occupies scarce space. Cacheable content is cached, lazy-loaded content stays out until needed, and compressible content gets compressed. | SYSTEM_PROMPT_DYNAMIC_BOUNDARY for prompt cache, fork-path cache optimization, four-stage compression |
| 5 | **Safety Layers Must Not Bypass Each Other** | The three-layer defense (classifier, hook, permission) can collaborate but no layer can override another's deny. A hook bug cannot silently approve a settings-denied operation. | resolveHookPermissionDecision(): Hook allow cannot bypass settings deny |
| 6 | **Ecosystem Key Is Model Awareness** | Giving the system ten plugins is useless if the model doesn't know when to use which. The extension mechanism's last mile is making the model see its own capability list. | MCP instructions injection, skill discovery, session-specific guidance in system prompt |
| 7 | **Productization Is About Day Two** | Day one is easy. The hard part is interruption recovery, dirty state cleanup, process leak handling, and session restoration. Without these, the product is just a demo. | runAgent.ts cleanup chain: transcript recording, MCP disconnection, hook cleanup, shell task termination, todo entry cleanup |

## Product Boundary

### In Scope

- Terminal-based conversational AI for software engineering tasks
- Local file system operations (read, write, edit, search)
- Shell command execution with permission controls
- Git integration (status, commit, push, PR workflows)
- Code review and security scanning
- Web search and content fetching
- MCP (Model Context Protocol) server integration
- Plugin and skill extensibility system
- Task and plan management
- Voice input (push-to-talk)
- Remote session sync with claude.ai
- Enterprise managed settings and policy enforcement
- Multi-agent orchestration (agents, teammates, coordinator mode)

### Out of Scope

- Graphical user interface (GUI) — CLI and terminal UI only
- Code hosting or repository management
- Continuous integration / continuous deployment (CI/CD) pipeline execution
- Cloud infrastructure provisioning or management
- Database administration
- Direct production deployment
- Model training or fine-tuning
