# System Roles & Permissions

## Role Definitions

### End User (Developer)
- **Definition**: The primary human user interacting with Claude Code through the terminal CLI
- **Responsibilities**:
  - Initiate conversations and provide prompts
  - Approve or deny tool execution permission requests
  - Configure settings, skills, hooks, and permissions
  - Review and accept code changes
- **Permission Scope**: Full control over all CLI features; can configure permission modes and rules
- **Relationships with Other Roles**: Directs Claude Assistant; configures Agent behavior; sets policies for all automated roles

### Claude Assistant
- **Definition**: The AI model (Claude) that processes user messages and invokes tools to accomplish tasks
- **Responsibilities**:
  - Understand user intent and provide responses
  - Select and invoke appropriate tools
  - Generate code, commit messages, PR descriptions
  - Manage conversation context and plan execution
- **Permission Scope**: Can invoke any registered tool subject to permission rules; cannot override user denials
- **Relationships with Other Roles**: Responds to End User; spawns Sub-Agents; invokes tools on behalf of user

### Sub-Agent
- **Definition**: A nested Claude instance spawned by the main Claude Assistant to handle specific sub-tasks in isolation
- **Responsibilities**:
  - Execute a scoped task with a separate token budget
  - Return results to the parent agent
  - Operate within the same or restricted permission context
- **Permission Scope**: Inherits parent permission rules; may have restricted tool access based on agent type
- **Relationships with Other Roles**: Spawned by Claude Assistant or Coordinator; reports back to parent

### Coordinator
- **Definition**: A specialized Claude instance that orchestrates multiple worker agents in parallel
- **Responsibilities**:
  - Break down complex tasks into parallelizable work items
  - Spawn and manage worker agents
  - Aggregate results from workers
  - Maintain a shared scratchpad for cross-worker knowledge
- **Permission Scope**: Can spawn agents and send messages; uses standard tool permissions
- **Relationships with Other Roles**: Orchestrates Sub-Agents; directed by End User

### Teammate Agent
- **Definition**: A persistent agent visible in the UI sidebar that can be assigned ongoing tasks
- **Responsibilities**:
  - Execute assigned tasks autonomously
  - Report progress and results
  - Respond to inter-agent messages
- **Permission Scope**: Inherits session permission rules; operates within its assigned scope
- **Relationships with Other Roles**: Managed by End User; may communicate with other Teammates

### Enterprise Administrator
- **Definition**: An organizational administrator who manages Claude Code deployment policies across teams
- **Responsibilities**:
  - Define managed settings and policies
  - Configure allowed/denied tools at the organization level
  - Set usage limits and compliance requirements
  - Manage MDM (Mobile Device Management) profiles
- **Permission Scope**: Can override user and project settings via policy; cannot directly interact with CLI sessions
- **Relationships with Other Roles**: Sets policies that constrain End User and Claude Assistant behavior

### MCP Server
- **Definition**: An external service that provides additional tools and resources via the Model Context Protocol
- **Responsibilities**:
  - Expose tools and resources to Claude Code
  - Handle tool invocation requests
  - Manage its own authentication and authorization
- **Permission Scope**: Tools subject to Claude Code permission rules and per-server allowlist/denylist
- **Relationships with Other Roles**: Extends Tool Execution System; configured by End User

### Hook Executor
- **Definition**: A deterministic automation process that runs user-defined shell commands in response to tool events
- **Responsibilities**:
  - Execute pre/post hook commands on tool use events
  - Return hook results (allow, block, or modify)
  - Run without AI involvement (deterministic)
- **Permission Scope**: Executes within the shell environment; not subject to AI permission rules
- **Relationships with Other Roles**: Triggered by Tool Execution System; configured by End User

## Permission Matrix

| Permission | End User | Claude Assistant | Sub-Agent | Coordinator | Teammate | Enterprise Admin | MCP Server | Hook Executor |
|------------|----------|-----------------|-----------|-------------|----------|-----------------|------------|---------------|
| Read files | ✅ | ✅ (with rules) | ✅ (inherited) | ✅ | ✅ | — | ✅ (via tool) | ✅ |
| Write/Edit files | ✅ | ✅ (with rules) | ✅ (inherited) | ✅ | ✅ | — | ✅ (via tool) | ✅ |
| Execute shell commands | ✅ | ✅ (with rules) | ✅ (inherited) | ✅ | ✅ | — | — | ✅ |
| Search files/content | ✅ | ✅ | ✅ | ✅ | ✅ | — | ✅ (via tool) | — |
| Web search/fetch | ✅ | ✅ (with rules) | ✅ (inherited) | ✅ | ✅ | — | — | — |
| Git operations | ✅ | ✅ (with rules) | ✅ (inherited) | ✅ | ✅ | — | — | ✅ |
| Spawn agents | ✅ | ✅ | ❌ | ✅ | ❌ | — | — | — |
| Configure settings | ✅ | ✅ (ConfigTool) | ❌ | ❌ | ❌ | ✅ (policy) | — | — |
| Manage permissions | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ (policy) | — | — |
| Override user settings | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | — | — |
| MCP server management | ✅ | ✅ (limited) | ❌ | ❌ | ❌ | ✅ (policy) | — | — |
| Hook management | ✅ | ✅ (ConfigTool) | ❌ | ❌ | ❌ | ✅ (policy) | — | — |
| Approve/deny tool use | ✅ | ❌ | ❌ | ❌ | ❌ | — | — | ✅ (block) |
| Voice input | ✅ | — | — | — | — | — | — | — |
| Remote session sync | ✅ | — | — | — | — | — | — | — |
